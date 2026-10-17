# Spec: Google Maps "Dead-Link Rescue" Scout

Version 1.1 · 2026-10-17. Written for any coding agent (Mode A) or browsing agent (Mode B).

**Changes in 1.1:** added an audio & creative category group with a "home turf" fit bonus, a remote
multi-city mode (outreach is by message, not calls), an optional `contact_channels` field, and
Appendix D, the rules for preview pages and first contact.

**Mission.** Find **10 real, currently operating local businesses** whose Google Maps listing sends
customers to a broken or missing website. Rank them by how badly they need a fix and how likely they
are to pay for one. A person must be able to check every result in **under 2 minutes** by clicking two
links.

Background: this is "Track A" of the plan in `expired-domain-flipping-audit.md`. We sell the business a
fix. **We never buy the business's domain.**

---

## 1. Inputs (fill these in before running)

| Input | Default | Notes |
|---|---|---|
| `TOWNS` | **must be filled in** | **Local mode:** 3–5 mid-size towns (50k–300k people) near the owner, e.g. `"Visalia, CA"`. **Remote mode** (outreach by email or DM, no calls): 10–20 metros, because audio businesses are sparse in any one town. |
| `CATEGORIES` | see §1.1 | Search phrases, not Places type IDs. |
| `MIN_REVIEWS` | 20 | Shows the business gets real customer traffic. |
| `MIN_RATING` | 3.5 | Drop listings that have no rating. |
| `MAX_SEARCH_CALLS` | 150 (remote mode: up to 400) | Hard cost limit on Places API calls (§4.1). Even 400 stays under the roughly 1,000 free Enterprise calls a month Google reportedly allows. |
| `TARGET_CONTENDERS` | 10 | |
| `MAX_PER_CATEGORY` / `MAX_PER_TOWN` | 3 / 4 | Keeps the list varied. |
| `EXCLUDED_CATEGORIES` | locksmith, garage door repair, towing, water damage restoration | These categories are full of fake lead-generation listings. |

### 1.1 Default categories and ticket size

| Ticket | Categories |
|---|---|
| High (10 pts) | roofing contractor, HVAC contractor, plumber, electrician, tree service, concrete contractor, fence contractor, auto body shop, foundation repair, epoxy flooring contractor, custom cabinet maker, med spa |
| Mid (6 pts) | auto repair shop, landscaper, house painter, pest control service, house cleaning service, carpet cleaning service, chiropractor |
| Low (3 pts) | dog groomer, hair salon, barber shop |
| **Audio & creative (home turf)** | recording studio, mastering studio, rehearsal studio, guitar repair shop, musical instrument repair, sound equipment rental (PA/backline), live music venue. Scored as mid ticket (6 pts) **plus a +5 fit bonus** (§4.5): the owner works in audio, which makes the pitch more credible. |

Default local run: about 8 categories × 4 towns = 32 queries × up to 3 pages, so at most 96 calls.
Default remote run: the 7 audio categories × 15 metros = 105 queries, so at most 315 calls with all pages
(usually fewer, since niche queries rarely fill 3 pages).

---

## 2. Rules that cannot be broken

1. **Zero fabrication.** Every fact in the output must come from an observation made *during this run*,
   recorded with a UTC timestamp. If something wasn't observed, write `"NOT OBSERVED"` and give the
   reason. Never fill gaps from memory, search snippets or guesses. A contender without complete
   evidence (§5.2) **is not a contender**.
2. **Allowed data sources:**
   - **Mode A:** the official Google Places API (New).
   - **Mode B:** a browsing agent viewing Google Maps interactively at human pace (§6).
   - **Not allowed:** automated scraping of google.com/maps or Google Search HTML, bought lead lists, or
     data from other directories presented as if it came from Google Maps.
3. **Hands off domains and people.** Never register, backorder, bid on or buy any domain. Never contact
   any business; outreach is done by hand by the project owner, outside this tool.
4. **Be polite to websites.** Check the homepage only, from at most 3 starting URLs per check (listed
   URL, `http://` fallback, `www`/bare-domain variant), each following at most 5 redirects. Use a descriptive
   User-Agent with a contact email, timeouts (connect 8 s, read 12 s) and at most 5 sites at once.
5. **Places data retention.** Keep `place_id` indefinitely. Delete other Places content (names, phones,
   ratings) within 30 days. Check the current Places API Policies before keeping anything longer.
6. **Report the funnel.** Record how many queries, listings, filter drops and labels there were (§5.1),
   so the hit rate can be audited and used for planning.

---

## 3. Pipeline

```
search (Places Text Search) → dedupe → qualify → check website → label
   → domain lookup (RDAP, only for domain-type problems) → score → pick top 10 (with variety caps)
   → re-check contenders → evidence pack → report
```

---

## 4. Step details

### 4.1 Search: Places API (New) Text Search

```
POST https://places.googleapis.com/v1/places:searchText
Headers:
  Content-Type: application/json
  X-Goog-Api-Key: $GOOGLE_MAPS_API_KEY           # from the environment; never hard-code or log it
  X-Goog-FieldMask: places.id,places.displayName,places.formattedAddress,places.googleMapsUri,
    places.businessStatus,places.primaryType,places.types,places.websiteUri,places.rating,
    places.userRatingCount,places.nationalPhoneNumber,places.pureServiceAreaBusiness,nextPageToken
Body:
  {"textQuery": "{category} in {town}", "pageSize": 20, "regionCode": "US", "languageCode": "en",
   "includePureServiceAreaBusinesses": true}
```

- **Pagination.** If `nextPageToken` comes back, repeat the *identical* body plus `"pageToken"`, for at
  most 3 pages per query. Google's docs say up to 60 results; some users report getting only 20, so
  handle a missing token gracefully.
- **Why `includePureServiceAreaBusinesses: true`.** Many contractors (plumbers, cleaners) have no
  storefront. Without this flag, the API returns only businesses with a physical location.
- **Cost.** `websiteUri`, `rating`, `userRatingCount` and `nationalPhoneNumber` push each call to the
  **Enterprise** Text Search price tier, about $35 per 1,000 calls after the free allowance. Google
  reportedly gives about 1,000 free Enterprise calls a month; confirm on Google's pricing page.
  - Enforce `MAX_SEARCH_CALLS`. Print a running count.
  - If the limit is hit, abort cleanly and still write partial results.
  - On 429 or 5xx, retry with exponential backoff, honoring `Retry-After`.

### 4.2 Dedupe and qualify

- Dedupe by `places.id`. Keep the first query that found each place, for traceability.
- **Keep** a place only if all of these hold:
  - `businessStatus == "OPERATIONAL"`
  - `userRatingCount >= MIN_REVIEWS`
  - `rating >= MIN_RATING`
- **Exclude chains.** A place is a chain if its website's registrable domain is shared by 2 or more
  places in the whole result set, or its name matches the configurable franchise list. Skip the
  shared-domain test for social and directory hosts and for `business.site` / `negocio.site`, since
  many unrelated businesses share those. Start the franchise list with Roto-Rooter, Mr. Rooter, Molly
  Maid, Merry Maids, Chem-Dry, Stanley Steemer, Terminix, Orkin, Midas, Meineke, Jiffy Lube, Great Clips,
  Supercuts, Sport Clips and The Joint. Corporate controls those websites.
- Exclude anything in `EXCLUDED_CATEGORIES`.
- Count every exclusion reason in the funnel.

### 4.3 Website check and labels

**Rules that need no fetch:**

| Condition on `websiteUri` | Label |
|---|---|
| Missing | `NO_WEBSITE` |
| Host ends with `.business.site` or `.negocio.site` | `DEAD_GOOGLE_SITE`. Google shut these sites down in 2024; links have errored since 2024-06-10. |
| Host is facebook.com, instagram.com, linktr.ee, yelp.com, nextdoor.com, tiktok.com, x.com, twitter.com, angi.com, homeadvisor.com, thumbtack.com, bbb.org | `SOCIAL_ONLY` |

**Fetch procedure** (when the fetch-free rules don't apply):

1. Resolve the hostname with `socket.getaddrinfo`.
   - **NXDOMAIN** (name not found) → `DNS_FAIL`. Also try the `www`/bare-domain variant first.
   - **SERVFAIL or timeout** → retry once after 60 s. If it fails again, label `CONNECT_FAIL`.
2. `GET` the listed URL, following at most 5 redirects. Record every hop as `(url, status)`.
   - **TLS error** on `https` → record it, then try `http://`.
3. Extract the page title, the visible text (strip script and style) and the URLs in `<script src>` and
   `<iframe src>`.

**Labels, in priority order.** The first match wins, but record *all* matching signals. Starter
signature lists are in Appendix A; keep them in a config file.

| # | Label | Rule |
|---|---|---|
| 1 | `BOT_PROTECTED` → **excluded**, counted as unknown | A WAF challenge: Cloudflare "Just a moment..." or `cf-chl`, Sucuri, Akamai "Access Denied … Reference #". **Never** call these broken. |
| 2 | `HIJACKED_SPAM` | (≥ 2 *distinct* spam-lexicon hits, **or** non-Latin letters ≥ 30% of the visible text on a US listing) **and** none of the business name's tokens (≥ 4 chars) appear in the title, H1 or first 2,000 chars. |
| 3 | `REGISTRAR_EXPIRED` | Registrar "this domain has expired / renew" page. The owner can probably still renew. |
| 4 | `PARKED_FOR_SALE` | Final host is a domain marketplace, **or** the page text matches a parking or for-sale phrase, **or** it loads a parking-network script. |
| 5 | `HOSTING_BROKEN` | Account-suspended page, an expired or disconnected site builder, a default server page, or "Index of /". |
| 6 | `DNS_FAIL` | From step 1. |
| 7 | `HTTP_ERROR` | Final status 404 or 410; 5xx (must be confirmed by a re-check ≥ 6 h later); 401 or 403 without a WAF signature (low weight). |
| 8 | `TLS_ERROR` | Listed URL is `https` and the certificate is expired, self-signed or for the wrong hostname. |
| 9 | `CONNECT_FAIL` | Refused, reset or timed out. **Confirmed only** if a second failure happens ≥ 6 h later. |
| 10 | `COMING_SOON` | A "coming soon", "under construction" or placeholder page. |
| 11 | `OUTDATED` (tier 3) | Site works but has no `<meta name="viewport">`, **or** only plain `http` works, **or** the copyright year is ≤ the current year − 4. |
| 12 | `OK` | None of the above. Not a contender. |

**Fallback when direct website requests are blocked.** Some sandboxes block outbound requests to
arbitrary sites. In that case, use the PageSpeed Insights API as a remote fetcher:

```
GET https://www.googleapis.com/pagespeedonline/v5/runPagespeed?url=<listed_url>&strategy=mobile&key=<key>
```

- When the page fails to load, the API returns an error whose message contains
  `Lighthouse returned error: <CODE>`.
  - `DNS_FAILURE` → `DNS_FAIL`
  - `ERRORED_DOCUMENT_REQUEST … (Status code: N)` → `HTTP_ERROR`
  - `INSECURE_DOCUMENT_REQUEST` → `TLS_ERROR`
  - `FAILED_DOCUMENT_REQUEST … net::ERR_…` → `CONNECT_FAIL`
- On success, use `lighthouseResult.finalDisplayedUrl` (for marketplace redirects) and the URLs in
  `audits["network-requests"]` (for parking-network scripts). Save `audits["final-screenshot"]` as
  evidence.
- **Verify these message formats on your first live call** and adjust the parser to match.
- Mark checks with `mode: "psi"`.

### 4.4 Domain lookup (RDAP)

Run this only for `DNS_FAIL`, `REGISTRAR_EXPIRED`, `PARKED_FOR_SALE`, `HIJACKED_SPAM` and
`HOSTING_BROKEN`.

- Get the registrable domain with `tldextract`, using its bundled suffix list
  (`TLDExtract(suffix_list_urls=())`) so it doesn't fetch anything live.
- Query `https://rdap.org/domain/<domain>`, or look up the registry's RDAP server in the IANA bootstrap
  file `https://data.iana.org/rdap/dns.json` (cache it for 24 h). At most 1 request per second.

**How to read the result** (check in this order):

| RDAP result | `domain_state` | What it means for the pitch |
|---|---|---|
| HTTP 404 | `NOT_REGISTERED` | The domain dropped and is available. The business should re-register it in its own account today. |
| `status` has "pending delete" | `PENDING_DELETE` | It can't be restored and drops in about 5 days. They should be ready to re-register it the moment it does. |
| `status` has "redemption period" | `REDEMPTION` | The owner can restore it at their registrar for a fee. Urgent. |
| `status` has "auto renew period", **or** the `expiration` event is in the past | `EXPIRED_GRACE` | The owner can still renew normally. Very urgent, and an easy fix. |
| `status` has "client hold" or "server hold" | `ON_HOLD` | Usually unpaid. They need to contact their registrar. |
| `registration` event < 365 days ago **and** the site shows parking or spam | `ACTIVE_RECENTLY_REGISTERED` | Someone else probably caught the domain when it dropped. The business needs a **new** domain. Don't negotiate with the new owner. |
| Anything else | `ACTIVE` | |
| Timeout, 429 or blocked | `RDAP_UNAVAILABLE` | Write "check lookup.icann.org manually". Never guess. |

For each domain, store the `status` list, the `registration` / `expiration` / `last changed` dates, the
registrar name, the RDAP URL that answered and `retrieved_at`.

### 4.5 Scoring (0–100)

`score = min(100, S (problem severity) + B (business strength) + T (ticket size) + U (urgency) + F (fit))`

F = 5 for the audio & creative categories in §1.1, otherwise 0.

| Component | Values |
|---|---|
| **S** (max 50) | `HIJACKED_SPAM` 50 · `DNS_FAIL` 45 · `DEAD_GOOGLE_SITE` 45 · `PARKED_FOR_SALE` 45 · `REGISTRAR_EXPIRED` 45 · `HOSTING_BROKEN` 40 · `HTTP_ERROR` 404/410 35, confirmed 5xx 30, 401/403 15 · `COMING_SOON` 30 · `TLS_ERROR` 25 · confirmed `CONNECT_FAIL` 25 · `SOCIAL_ONLY` 15 · `NO_WEBSITE` 12 · `OUTDATED` 8 |
| **B** (max 30) | By review count: 20–49 → 10 · 50–149 → 18 · 150–499 → 24 · 500+ → 26. Rating bonus: ≥ 4.5 → +4 · 4.0–4.49 → +2 |
| **T** (max 10) | From §1.1: high 10 · mid 6 · low 3 |
| **U** (max 10) | `EXPIRED_GRACE` 10 · `REDEMPTION` 10 · `ON_HOLD` 8 · `NOT_REGISTERED` 8 · `PENDING_DELETE` 6 · `ACTIVE_RECENTLY_REGISTERED` 4 · otherwise 0 |

Tiers: **A** ≥ 75 · **B** 60–74 · **C** < 60.

### 4.6 Picking the top 10

1. Only places with S ≥ 25 are eligible.
2. Sort by score, then review count, then S.
3. Fill the list, skipping any place that would break `MAX_PER_CATEGORY` / `MAX_PER_TOWN` or whose
   evidence is incomplete (§5.2).
4. If fewer than 10 qualify, top up with `SOCIAL_ONLY` and `NO_WEBSITE` places, marked **tier 3**. Never
   lower the evidence standard to fill the list.

### 4.7 Re-check

Right before writing the report, run the website check again for every contender.

- If the label no longer matches (the site came back), drop the place, list it under "False positives
  caught", and promote the next one.
- `CONNECT_FAIL` and 5xx must have two failing checks at least 6 hours apart.

---

## 5. Output

Write all of these to `out/`:

| File | Contents |
|---|---|
| `run_meta.json` | Inputs, start and end times, tool version, API call counts, fetch mode, **funnel counts** |
| `checked.csv` | Every qualified place, with its label, signals, score and domain state |
| `contenders.json` | Top 10 in the §5.2 format |
| `contenders.md` | Human report (§5.3) |
| `screenshots/<place_id>.png` | Optional: Playwright, mobile viewport, 15 s timeout |

### 5.1 Funnel counts (required in `run_meta.json`)

`queries_run, api_calls, places_returned, unique_places, operational, passed_review_rating,
excluded_chain, excluded_category, websites_checked, label_counts{...}, rdap_lookups,
contenders_selected, false_positives_on_recheck`

### 5.2 Contender format (every field required; use `"NOT OBSERVED"` only where allowed)

```json
{
  "rank": 1, "score": 87, "tier": "A",
  "place": {
    "id": "ChIJ...", "name": "...", "maps_url": "https://maps.google.com/?cid=...",
    "address": "...", "phone": "...", "primary_type": "plumber",
    "rating": 4.6, "review_count": 132, "business_status": "OPERATIONAL",
    "service_area_business": true
  },
  "found_by": {"category": "plumber", "town": "Visalia, CA"},
  "website": {
    "listed_url": "https://example-plumbing.com/",
    "registrable_domain": "example-plumbing.com",
    "label": "PARKED_FOR_SALE",
    "signals": ["final_host=hugedomains.com", "text:'this domain is for sale'"],
    "checks": [
      {"checked_at": "2026-10-18T17:02:11Z", "mode": "direct",
       "chain": [{"url": "https://example-plumbing.com/", "status": 302},
                 {"url": "https://www.hugedomains.com/domain_profile.cfm?d=example-plumbing.com", "status": 200}],
       "final_url": "https://www.hugedomains.com/domain_profile.cfm?d=example-plumbing.com",
       "error": null, "title": "example-plumbing.com is for sale | HugeDomains",
       "text_snippet": "first 300 chars of visible text",
       "screenshot": "screenshots/ChIJ....png"}
    ],
    "reproduced_on_recheck": true
  },
  "domain": {
    "state": "ACTIVE_RECENTLY_REGISTERED", "status": ["client transfer prohibited"],
    "registration": "2026-08-30", "expiration": "2027-08-30", "last_changed": "2026-09-02",
    "registrar": "...", "rdap_url": "https://rdap.verisign.com/com/v1/domain/example-plumbing.com",
    "retrieved_at": "2026-10-18T17:02:15Z"
  },
  "pitch_angle": "see Appendix B",
  "contact_channels": [
    {"type": "phone", "value": "(559) 555-0100", "source": "places.nationalPhoneNumber"},
    {"type": "instagram", "value": "@exampleplumbing", "source_url": "https://www.instagram.com/exampleplumbing/"}
  ],
  "human_verified": null
}
```

`contact_channels` is optional. Each entry must name where it was seen (a Places field or a public page
URL). Never guess email addresses or invent handles.

Required, never `"NOT OBSERVED"`: `place.id`, `place.maps_url`, `place.name`, `place.review_count`,
`website.listed_url` (or `null` for `NO_WEBSITE`), `website.label`, and, for every label except
`NO_WEBSITE`, at least one `checks[]` entry with `checked_at` plus `chain` or `error`. The `domain` block
is required for the labels listed in §4.4.

### 5.3 `contenders.md` layout

1. Header: run date, towns, categories, fetch mode.
2. Funnel table (§5.1). Include the line "X of Y qualified listings had a website problem".
3. **Top 10 table:** Rank · Business · Town · Category · Reviews (rating) · Problem · Domain state ·
   Score · Maps link · Website link.
4. One evidence card per contender: the check results, signals, domain lookup and pitch angle (Appendix B).
5. "False positives caught on re-check" and "Near misses" (up to 5).
6. "Not checked / limitations" (for example, RDAP being unavailable).

---

## 6. Mode B: rules for a browsing agent (no code)

Prefer Mode A. Use Mode B only for an agent that browses **interactively on the owner's behalf, the way
a person would**: no scraping tools, no bulk export, at most about 150 listings viewed per run.

1. For each (category, town): search Google Maps for `{category} in {town}`. Open listings that show
   **≥ MIN_REVIEWS reviews**, **≥ MIN_RATING**, and are not marked "Permanently closed" or "Temporarily
   closed".
2. Copy the listing's **Share link** as `maps_url`, then tap **Website** and copy the URL exactly as
   `listed_url`.
3. Note what loads (error text, final URL, page title, the first line of visible text) and take a
   screenshot. Label it with the same §4.3 labels and Appendix A phrases. **If unsure, label it `OK` and
   move on.**
4. For domain-type labels, look the domain up at `https://lookup.icann.org/`. Record status, dates and
   registrar, and take a screenshot.
5. Score with §4.5, pick with §4.6, re-check with §4.7 (the six-hour rule applies).
6. Produce the same `contenders.md` (§5.3), funnel counts included. Leave out any listing you didn't
   personally open in this run.

---

## 7. Acceptance criteria

**Unit tests.** Use fixture HTML or recorded responses. Each case must produce the stated label.

| Fixture | Expected |
|---|---|
| GoDaddy parked page ("parked free, courtesy of GoDaddy" or `wsimg.com/parking-lander`) | `PARKED_FOR_SALE` |
| Redirect chain ending on `hugedomains.com` | `PARKED_FOR_SALE` |
| Sedo page ("This domain may be for sale" + `sedoparking.com` script) | `PARKED_FOR_SALE` |
| Registrar page "This domain name expired on Oct 2, 2026 … renew" | `REGISTRAR_EXPIRED` |
| cPanel "This Account has been suspended" | `HOSTING_BROKEN` |
| Wix "Looks like this domain isn't connected to a website yet!" | `HOSTING_BROKEN` |
| Apache "Apache2 Ubuntu Default Page" | `HOSTING_BROKEN` |
| "Coming Soon" placeholder | `COMING_SOON` |
| Cloudflare "Just a moment..." with status 403 | `BOT_PROTECTED` (**must not** count as broken) |
| Plumber's domain showing "slot gacor … situs judi" | `HIJACKED_SPAM` |
| Page that is mostly CJK text, no business-name tokens | `HIJACKED_SPAM` |
| Real plumber site mentioning "Casino Road" once | `OK` (one spam hit isn't enough) |
| Working site with no viewport and "© 2016" | `OUTDATED` |
| Modern working site | `OK` |
| Simulated NXDOMAIN | `DNS_FAIL` |
| Final status 404 | `HTTP_ERROR` |
| `https://acme.business.site` | `DEAD_GOOGLE_SITE` (no network call made) |
| `https://facebook.com/acme` | `SOCIAL_ONLY` |

**RDAP parser tests.** 404 → `NOT_REGISTERED`. "redemption period" → `REDEMPTION`. "pending delete" →
`PENDING_DELETE`. "auto renew period" or a past expiration → `EXPIRED_GRACE`. "client hold" →
`ON_HOLD`. Registered 40 days ago plus a parked page → `ACTIVE_RECENTLY_REGISTERED`.

**Scoring and selection tests.** Fixed inputs give exact scores. The variety caps are enforced. Places
with incomplete evidence are skipped.

**Offline dry run.** `--dry-run fixtures/` runs end to end with recorded Places and HTTP responses and
no API key, and writes all §5 files.

**Cost limit test.** Going over `MAX_SEARCH_CALLS` aborts cleanly and still writes partial results.

**Live run passes when:**
- 10 contenders (or fewer, with an explanation), each with complete §5.2 evidence
- funnel counts present
- no API key in any output or log

---

## 8. Implementation notes (Python)

- Python 3.11+. Dependencies: `requests`, `tldextract`, `PyYAML`, `pytest`. Optional: `playwright` for
  screenshots.
- CLI:
  ```
  python scout.py --config config.yaml --out out/ [--fetch-mode direct|psi] [--dry-run fixtures/]
                  [--max-search-calls 150] [--recheck-only out/contenders.json]
  ```
- Read `GOOGLE_MAPS_API_KEY` from the environment. The PageSpeed fallback uses the same key with the
  PageSpeed Insights API enabled. Restrict the key to those two APIs in Google Cloud.
- Store signature lists, the franchise list and the category tiers in `config.yaml`, so they can be
  changed without touching code.
- Log every request (method, URL, status, duration) to `out/requests.log`, never including the key.

---

## 9. What happens after the run (verification)

The owner, or Claude with API access, checks each contender:

1. Open `maps_url`: does the name, review count and **Website** link match?
2. Open `listed_url` on a phone: is the problem really there?
3. For domain-type labels, check `lookup.icann.org`: does the state match?
4. Set `human_verified` to true or false, with a note.

**Precision = verified ÷ contenders.** Below 7/10 means the run fails and the rules or signatures need
fixing. Send back `contenders.json`, `contenders.md`, `run_meta.json` and the screenshots for review.

---

## 10. Known limitations

- Text Search returns at most about 60 results per query, ranked by Google's own relevance. Vary the
  wording if a town looks thin.
- The listing's website field can be stale or wrong. That's the opportunity, but it also means the
  business may already have a new site the listing doesn't show. Mention it on the call.
- Sites go down temporarily, which is why the re-check rules exist.
- WAF challenges and geo-blocking can make healthy sites look broken. When in doubt, the result is
  `BOT_PROTECTED` or unknown, never "broken".
- RDAP servers rate-limit. `RDAP_UNAVAILABLE` is an acceptable answer; a guessed status is not.

---

## Appendix A: Starter signatures (case-insensitive; keep in config)

**Domain marketplace hosts (final URL):**
hugedomains.com, afternic.com, sedo.com, dan.com, atom.com, squadhelp.com, brandbucket.com,
undeveloped.com, buydomains.com, domainmarket.com, efty.com, uniregistry.com. Also count godaddy.com,
namecheap.com, dynadot.com and spaceship.com, but only when the path matches
`forsale|/market|domain-?search`.

**Parking-network script hosts:**
sedoparking.com, parkingcrew.net, bodis.com, above.com, `img1.wsimg.com/parking-lander`,
parkingpage.namecheap.com

**For-sale / parked phrases (regex):**
`(this|the) domain (name )?(is|may be) for sale` · `buy this domain` · `make an offer` (combined with
`domain`) · `get this domain` · `domain (is )?parked` · `parked free, courtesy of` ·
`inquire about this domain`

**Registrar expired (regex):**
`domain (name )?(has )?expired` · `expired on [a-z]+ \d{1,2},? \d{4}` · `renew (it|this domain|now)`
combined with `expired` · `if you are the (owner|registrant)` combined with `renew`

**Hosting broken (phrases):**
`this account has been suspended` · `account suspended` · `website expired` ·
`this site (is|has been) (temporarily )?unavailable` · `looks like this domain isn't connected to a
website yet` · `sorry, this store is currently unavailable` · `only one step left` ·
`this site has been archived or suspended` · `future home of something quite cool` ·
`apache2 ubuntu default page` · `welcome to nginx` · `iis windows server` · title `index of /` · title
`it works!`

**Coming soon:** `coming soon` · `under construction` · `site under maintenance` · `launching soon`

**WAF / bot protection (exclude):** title `just a moment...` · `cf-chl` · `attention required! |
cloudflare` · `sucuri website firewall` · `access denied` combined with `reference #`

**Spam lexicon (count distinct hits):**
- Gambling: casino · slot gacor · slot online · judi · togel · situs · bandar · taruhan · maxwin · sbobet
  · poker online · sportsbook
- Pharma: viagra · cialis · levitra · online pharmacy · no prescription
- Adult: porn · xxx · escort
- Other: replica watches

These are starting points. Extend them from real false negatives, and add a fixture for every change.

---

## Appendix B: Pitch angles by finding

| Finding | Angle (used later by hand, not by this tool) |
|---|---|
| `REGISTRAR_EXPIRED` / `EXPIRED_GRACE` / `REDEMPTION` | "Your domain expired on {date}. If it isn't renewed soon you could lose it for good. Renew it in your registrar account today. I can help with that and get the site working again." Lead with the free warning. |
| `NOT_REGISTERED` / `PENDING_DELETE` | "Your old domain lapsed. Re-register it **in your own account**, and I'll walk you through it and put a working site on it." |
| `ACTIVE_RECENTLY_REGISTERED` / third-party `PARKED_FOR_SALE` | "Someone else now owns your old domain and shows customers a for-sale page. Don't pay them. Let's move you to a new domain and update your Google listing today." |
| `HIJACKED_SPAM` | "Your Google listing currently sends customers to a gambling/spam site. Let's change the link today and set up a new site." The most urgent case. |
| `DEAD_GOOGLE_SITE` | "Google shut down its free business.site websites in 2024. Your listing still points to one that no longer works." |
| `HOSTING_BROKEN` / `HTTP_ERROR` / `TLS_ERROR` / `CONNECT_FAIL` | "Your website shows {error or warning} to customers right now, especially on phones." |
| `COMING_SOON` | "Your listing's website is still a 'coming soon' page." |
| `SOCIAL_ONLY` / `NO_WEBSITE` (tier 3) | "A simple site with click-to-call turns Maps visitors into calls better than a Facebook page." |

---

## Appendix C: Example `config.yaml`

```yaml
mode: remote            # local | remote
towns: ["<Metro 1, ST>", "<Metro 2, ST>", "<Metro 3, ST>"]   # local: 3–5 nearby towns; remote: 10–20 metros
categories:
  high: [roofing contractor, HVAC contractor, plumber, electrician, tree service, concrete contractor, fence contractor, auto body shop, foundation repair, epoxy flooring contractor, custom cabinet maker, med spa]
  mid:  [auto repair shop, landscaper, house painter, pest control service, house cleaning service, carpet cleaning service, chiropractor]
  low:  [dog groomer, hair salon, barber shop]
  home_turf: [recording studio, mastering studio, rehearsal studio, guitar repair shop, musical instrument repair, sound equipment rental, live music venue]
fit_bonus_categories: home_turf
run_categories: [recording studio, mastering studio, rehearsal studio, guitar repair shop, musical instrument repair, sound equipment rental, live music venue]
excluded_categories: [locksmith, garage door repair, towing, water damage restoration]
min_reviews: 20
min_rating: 3.5
max_search_calls: 400   # local mode: 150
target_contenders: 10
max_per_category: 3
max_per_town: 4
user_agent: "DeadLinkScout/1.0 (+mailto:you@example.com)"
fetch:
  connect_timeout_s: 8
  read_timeout_s: 12
  max_redirects: 5
  concurrency: 5
recheck_min_hours: 6
```

---

## Appendix D: Preview pages and first contact (the outreach step, done by hand)

The scout never builds pages or sends messages. These rules are for the owner's next step, when
pitching a business with a ready-made preview page instead of a phone call.

**What a preview page may contain**
- Facts the business publishes itself: name, phone, service area or address, hours, the services it
  offers.
- Your own copy and layout, and neutral stock imagery or none.
- A **Google Maps Embed** iframe (`/maps/embed/v1/place?q=place_id:<id>`) to show the real listing and
  rating. It's free with unlimited usage and is the permitted way to show Google's data.

**What it must not contain**
- **Copied Google reviews or photos.** Places API policy allows storing only place IDs, and reviews and
  photos must be shown with their author attribution. Copying them into a static page breaks both rules,
  and the review text belongs to its authors.
- The business's logo or anything implying the page is theirs.

**How to host it**
- Use an unguessable URL, a `<meta name="robots" content="noindex,nofollow">` tag and a robots.txt
  disallow, so the page never shows up in search results.
- Show a visible banner: "Unofficial preview prepared for {Business} by {your name}. Not affiliated.
  Removed on request."
- Delete it after 14 days, or immediately if the business asks.
- No forms that collect customer data. Click-to-call goes to the business's real number.

**First contact**
- Use email (from a public profile or directory page) or an Instagram/Facebook DM. Send **one** message
  plus at most one follow-up, with an opt-out line.
- **Don't cold-message on WhatsApp.** WhatsApp requires the recipient's opt-in for business messages, and
  unsolicited messages get numbers restricted.
- Follow the cold-email rules where the business is located. CAN-SPAM in the US: honest subject line,
  postal address, working opt-out. EU and UK rules are stricter, especially for sole traders.
- Lead with the problem and the screenshot. The preview link is proof of work, not pressure. Price
  stays flat ($250–$400) and the business owns every account it ends up using.
