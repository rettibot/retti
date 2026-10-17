# Expired-Domain Flipping: Audit and Upgraded Plan

Prepared 2026-10-17. Goal: fund an Audeze MM-520 by 2027-01-31.

> **What I could and couldn't verify.** This environment's network policy blocked live RDAP/WHOIS and
> Wayback Machine lookups (`data.iana.org`, `rdap.verisign.com`, `rdap.org`, `web.archive.org`). So the
> *current registration status* of the ten domains is **unconfirmed**. The verdicts below rely on the
> agent's own numbers plus each name's public footprint, which I checked with web search. Every claim the
> agent tied to a "study", "guide" or "case study" was searched for, and the result is recorded below.

---

## 1. Bottom line

1. **About half the method is solid SEO practice and half is unverifiable.** The anchor-text and Wayback
   checks, the `site:` index check and 1:1 redirect mapping are good practice. Other parts don't hold up:
   the "October 2026 DomCop study", the "$100K case study", the Kindwise link to classif.io and the
   claimed "Expired Domain Abuse classifier" mechanism didn't turn up anywhere I searched.
2. **None of the ten names is a confident buy.** Three should never be bought (heatherschoice.com,
   pruschfarmpark.org, podcurator.io). Four more are poor bets. Two need checks first. One fits a
   legitimate approach, with modest upside. Several picks also fail the agent's *own* filters.
3. **The numbers can't reach the goal.** The plan needs about 5–10 sales at $249–$499 by Jan 31.
   Domain portfolios typically sell 1–4% of their names per *year*. Ten names over 106 days works out to
   about **0.03–0.12 expected sales**.
4. **The better plan keeps the research skill but sells something else: fix the problem instead of
   selling the domain.** The strategy's best signal is local businesses whose Google listing points at a
   dead domain. Charge those businesses to fix their own web presence. Don't buy their old domain and try
   to sell it back to them, which is the pattern trademark law treats as cybersquatting.
5. **Keep a small domain side bet ($100 cap)** using the stricter checklist in §7, and **lower the
   target** by watching for Audeze B-Stock and Black Friday deals.

---

## 2. The target

| Item | Figure |
|---|---|
| MM-520 US price | **$1,699** at US retailers. Stereonet's launch article said $1,799, price still to be confirmed. |
| Sales tax | about $0–$170 depending on your state |
| **Amount to raise** | **about $1,700–$1,870** |
| Time available | Oct 17 → Jan 31 = 106 days (about 15 weeks) |
| Pace needed | about **$120/week** |
| Stock | headphones.com lists it as a **preorder**, so aim to have the money by about **Jan 15** to allow for lead time |

The MM-520 is Audeze's open-back planar successor to the MM-500 (90 mm drivers, SLAM). It was announced
around June 2026 and went on sale in Japan on Sept 1, 2026.

**What the agent's plan would need.** Its BINs are $249–$499. Afternic takes 15% if the domain is on
GoDaddy aftermarket nameservers, 25% if not, plus 5% with Boost, with a $15 minimum. That leaves about
**$187–$424 per sale**, so you'd need **5–10 sales**. Typical sell-through is 1–4% of names per year, so:

```
10 names × (1%–4% per year) × (106 / 365) ≈ 0.03–0.12 expected sales by Jan 31
```

Even names that sold ten times better than average would produce about one sale.

---

## 3. Audit of the six discovery methods

| # | Method | What checks out | What doesn't | Verdict |
|---|---|---|---|---|
| 1 | **Day 2–30 post-drop window** | Peer-reviewed research agrees with the general idea. Only about 10% of expired domains are ever re-registered, and most of those on the day they're released (WWW 2018). About 10% of deleted .com names are re-registered the same day (USENIX Security 2017). | I couldn't find the **"October 2026 DomCop study of 500,000 domains" (66% / 17%)** anywhere. Treat those numbers as made up until someone produces a URL. | **Keep the idea, drop the numbers.** A domain with strong metrics that is still unregistered after N days is a *warning*. The drop-catching bots saw the same metrics and passed. |
| 2a | **TF/CF between 0.6 and 1.2** | A common rule of thumb for spotting link spam. | It isn't a standard, and TF above CF isn't bad in itself. **The agent's own picks break it:** foxs.io (1.31), heatherschoice.com (2.00), pruschfarmpark.org (2.58). | Fine as a guide, but the agent didn't apply it. |
| 2b | **Topical Trust Flow ≥ 20** | Majestic does sort the web into roughly 800 topics ("nearly 1,000" at launch). | convertonline.io (TF 19) and finlaw.io (TF 10) effectively can't meet it, because topical scores break down the overall Trust Flow. The claim that it stops Google's "Expired Domain Abuse classifier" from "zeroing out links" is **made up**. Google's policy (March 2024) is about *what you publish* on a repurposed domain, not Majestic metrics. Google says reusing an old domain for a new, original, people-first site is fine. | Strict, applied inconsistently, and the explanation behind it is false. |
| 2c | **Moz Spam Score ≤ 5%** | Acceptable as a strict cutoff. Moz counts 1–30% as "low". | It measures features of the site itself, not backlink toxicity. pruschfarmpark.org (6%) fails it. | Keep, low weight. |
| 2d | **Anchor-text limits and clean Wayback history** | Good practice. | It leaves out the most useful quick check: **backlinks per referring domain**. convertonline.io has about 130 per domain and abatherapistjobs.com about 133, a sign of sitewide or syndicated links. | Keep, and add the ratio check (§7). |
| 3 | **Wikipedia citations** | **Verified:** Pew Research (May 2024) found 54% of Wikipedia pages have at least one dead reference link, and 11% of all references are inaccessible. | Wikipedia external links are **nofollow**. Editors mark repurposed domains `url-status=usurped`, which **hides the link**, and bots swap in archive.org copies. A blacklisted domain can't be cited at all. What's left is a little referral traffic and a sales talking point, and the citation disappears once the content changes. | The statistic is real, but the value is overstated. |
| 4 | **Orphaned Google Business Profiles** | **Verified:** the CatchDoms MCP server exists and flags `has_gmb`. That feature is on the paid Pro and Authority tiers only. | If the business is still operating, registering its name to sell back to it is the textbook cybersquatting pattern. That risks a UDRP complaint, and US ACPA statutory damages run **$1,000–$100,000 per domain**. If the business has closed, sending its customers somewhere else is deceptive, and the value vanishes once the listing is updated. | **Strong signal, wrong way to make money from it.** Use it for Track A (§6). |
| 5 | **Natural decay check and 1:1 redirects** | `site:` checks, gradual decline vs. a cliff, and 1:1 URL mapping are all sound. Google treats mass redirects to the homepage as **soft 404s** (Mueller). | I couldn't find the "DomCop $100K Case Study". SimilarWeb **doesn't show data for sites under 5,000 visits/month** and is least accurate for small sites. For a dropped domain, any "visits" figure describes the past. | The method is sound; the traffic data is weak. |
| 6 | **Retired API/SaaS sweeps (RDAP 404)** | API Evangelist provider pages do exist. | They're auto-generated and contradict each other. Classif.io is described as both a fashion image API and a data-classification platform. It isn't a retirement tracker. **Security risk:** dead SaaS and startup domains keep receiving traffic from installed software. Re-creating email on them can unlock former employees' SaaS accounts through Google sign-in (Truffle Security, Jan 2025). watchTowr's $20 re-registered domains received check-ins from more than 4,000 abandoned backdoors, and a stale .mobi WHOIS domain was queried by more than 135,000 systems. | **Avoid** unless you've confirmed the domain has no software footprint. **Never enable email on an expired domain.** |

---

## 4. Audit of the top-10 list

| # | Domain | Verdict | Why |
|---|---|---|---|
| 1 | convertonline.io | **Avoid** | The product appears to have been live recently. A Google Workspace add-on, "ConvertOnline – Fast & Secure File Converter", was updated Nov 14, 2025 and asks for Drive access. The "8,477 visits" are most likely that product's users. The agent's plan (a converter on the old URLs to "retain the visits") would have people uploading files to a stranger posing as a service they trust. It also has about 130 backlinks per referring domain, fails TTF ≥ 20, and .io renews at about $50/yr. |
| 2 | abatherapistjobs.com | **Verify first** | A Dec 2025 press release (TechBullion, widely syndicated) promoted it as an active ABA-only job board. So it was an operating business recently, and syndicated releases like that probably explain the "57,037 backlinks / 429 RDs" (about 133:1). A Dec 2025 launch also doesn't fit the claimed "5-year archive" (unless it was relaunched on an older domain). The name is descriptive, so trademark risk is low. Consider it only if it's truly available, the links are editorial, and you can name 10 or more buyers. |
| 3 | yoursoccerhome.com | **Verify first** | The listing contradicts itself. "In Namecheap renewal grace" means the owner can still renew, so a $21 grace-period listing isn't a guaranteed purchase. It has 2,519 referring domains but only DR 28, so most of them are junk. The "coaching" link cited comes from a parkour athlete's page, which is off-topic. Web search found no trace of the site. Realistic value is $50–$150 to an SEO buyer, not $449. |
| 4 | classif.io | **Avoid** | The only evidence is auto-generated API Evangelist pages that contradict each other. Nothing links it to Kindwise. If it was an API domain, old clients may still call it. It's .io, the brand is weak, and a $449 BIN is unrealistic. |
| 5 | foxs.io | **Skip** | It isn't "rare": there are 456,976 four-letter combinations, and "foxs" is a misspelling. DR 33 next to Moz DA 12 on 51 referring domains is the kind of mismatch inflated links produce. TF/CF is 1.31, which fails the agent's own rule. "981 visits" is below SimilarWeb's own 5k display threshold. Expect wholesale prices (tens of dollars), not $399. |
| 6 | pruschfarmpark.org | **Do not buy** | The name belongs to Emma Prusch Farm Park, a City of San Jose park, and its 501(c)(3) foundation (est. 1982), which was in a public dispute with the city in 2024. Visitors are looking for the real park. Turning the domain into a gardening guide to flip is almost exactly an example in Google's expired-domain-abuse policy (a former charity site repurposed for commercial content). It also fails the agent's TF/CF and spam filters. |
| 7 | finlaw.io | **Skip** | Active, unrelated businesses already use "FinLaw" (an Australian financial-services legal locum firm and a LatAm venture). So the realistic buyers already have a name, and pitching cold to RegTech founders will get few replies. TF 10 fails TTF ≥ 20. |
| 8 | podcurator.io | **Do not buy** | A Chrome Web Store extension, "PodCurator: Find Podcasts for Any Page", points to podcurator.io and reads the page the user is viewing. A directory listing was added Oct 2025. Owning a domain that installed software still calls means receiving those users' traffic. The obvious buyer is the original developer, which turns any sale into holding their domain to ransom. |
| 9 | heatherschoice.com | **Do not buy** | Heather's Choice is a known brand (Shark Tank Season 15). Its wind-down email says "no final decisions have been made regarding the long-term future of the brand or its assets," so the brand may still be sold. Running outdoor-food affiliate content on it trades on the brand's goodwill in the *same niche*, the classic UDRP/ACPA situation. A clean USPTO search doesn't fix that, because unregistered (common-law) trademark rights still count. |
| 10 | HoustonFoundationPros.com | **Legitimate approach, modest odds** | This is the only pick that fits a legitimate approach: a generic city + service .com sold by contacting businesses directly. Availability is unconfirmed. "Pros"-suffix names are weak, and contractors with budgets usually already own a brand domain. A 30-second account push only works if the buyer uses the same registrar. Domain investors self-report about 1–2 sales per 100 well-targeted emails. Use it as a template for Track B, not as a sure sale. |

---

## 5. Problems across the whole plan

1. **The closeout problem.** A closeout is a name that sat through a 10-day public auction while
   drop-catchers and PBN buyers looked at the same DR, TF and traffic numbers, and nobody bid. Strong
   metrics at a closeout price mean something is wrong: inflated links, a trademark, software still
   calling the domain, or a penalty. The agent never asks why everyone else passed.
2. **Citations that look real but aren't.** Labels like "(DomCop Anchor & TF/CF Analysis)" come with no
   URL. Several key ones (the October 2026 study, the $100K case study, Kindwise as builder of
   classif.io, a "Venture North Group" connection to Heather's Choice) didn't turn up in any search.
3. **It breaks its own rules.** Three of the eight picks with metrics fail its TF/CF range, two can't
   meet its TTF ≥ 20 rule, and one fails its spam-score cap.
4. **The traffic figures aren't current traffic.** SimilarWeb doesn't display sites under 5,000
   visits/month, and its small-site estimates are its worst. Once a domain drops, the content and most of
   the visits are gone. Any remaining hits are mostly bots, old bookmarks and, for software domains, apps
   still calling home.
5. **Sell-through, fees and lock-ups.**
   - Sell-through is 1–4% per year.
   - Fees: Afternic takes 15–25% (+5% with Boost). Flippa charges a listing fee plus about 10% on small
     sales.
   - .io renews at about $38–$52/yr.
   - The 60-day post-registration transfer lock still applies. ICANN is moving to 30 days, but that
     isn't in effect yet. Sales to buyers at other registrars wait or require escrow.
   - .io carries political risk. The Chagos handover was frozen as of April 2026, so .io isn't being
     retired, but buyers price that uncertainty in.
6. **Legal and security risks** (detailed in §3–4): trademarks, public-interest names, software that
   still calls the domain, email on recycled domains.

---

## 6. The upgraded plan

### Track A: Dead-link rescue for local businesses (main track, target $1,200–$1,800)

This is method 4 with the same signal and a different product. You sell the business a fix for its
broken listing. You don't buy its domain.

**Finding prospects.** In Google Maps, search `[service] near [town]` across 3–5 towns and 3 categories.
Tap **Website** on each listing and log the ones that show:

- "This site can't be reached" or a DNS error (domain expired)
- a registrar parking page or a "this domain is for sale" page
- a spam, casino or foreign-language page (someone else drop-caught the domain)
- a dead `*.business.site` link. Google shut down Business Profile websites in 2024, and those links
  have shown errors since June 10, 2024. Many listings still carry one.
- an SSL warning, a 404, or a site that's badly broken on mobile

Qualify the ones worth calling: **20+ reviews, still open, owner responds to reviews.**

Optional speed-up: CatchDoms (paid tiers) flags dropped domains that are still attached to a Google
listing. Contact the business; don't buy the domain.

**Offer.** These are my suggested prices; adjust for your area.

| Package | Price | Includes |
|---|---|---|
| Fix | $150–$250 | Get their domain renewed or registered *in their own account*, a simple 1-page site, and the listing's Website field corrected |
| Rescue site | $350–$600 | 3–5 pages (services, reviews, click-to-call, hours, map), fast on mobile, Search Console set up |
| Care plan (optional) | $20–$40/mo | Hosting and builder upkeep, small edits, link monitoring |

Delivery tools: Carrd, Google Sites, Wix, Squarespace or WordPress. The client pays the builder and
registrar directly and owns every account. You work with Manager access to the listing.

**Rules that keep this clean and legal**

- Never register a business's name on spec. If its old domain is still in grace or redemption, tell the
  owner to renew it; that's cheapest for them. If it's available, they register it themselves or you do
  it for them in writing and transfer it at cost.
- If someone else owns the old domain (it's parked or for sale), don't negotiate for the business
  without written authority. Set them up on a new domain.
- Emails must follow CAN-SPAM: honest subject line, your postal address, a working opt-out.
- Get a one-paragraph written agreement for each job, keep receipts, and remember this income is
  taxable.

**Volume math (estimates; track your own rates).** You need 4–6 jobs at about $400. Assuming 3–5% of
contacted owners buy, that's about 100–150 contacts, or **10–15 per week**. Phone calls and walk-ins
convert better than email.

### Track B: Domain side bet, tightened (cap: $100 total)

Only two kinds of name qualify:

1. **Generic city + service .com names.** Pick mid-size metros and high-ticket services (foundation
   repair, roofing, tree service, water damage). Hand-register them or buy closeouts at $12 or less. Price
   at $300–$900 with a payment-plan option, and pitch 20–30 local businesses per name. Keep them at
   GoDaddy and list on Afternic with GoDaddy aftermarket nameservers (15% commission), or invoice through
   Escrow.com.
2. **Deleted (available) domains** that pass every item in the §7 checklist **and** have a buyer you can
   name. If the only buyer is a PBN builder, pass.

**Never buy** brands (operating or defunct), government, school, charity or park names, domains that
software still calls, anything with CJK/casino/pharma history, or .io names without a strong reason.

**Stop rule:** if you've had no inquiries by Dec 31, stop buying and let the names lapse. Don't pay
about $50 to renew a .io you couldn't sell.

**Expectation:** 0–1 sales by Jan 31. Treat it as practice and a small upside, not the plan.

### Track C: Lower the target

- Audeze's 2023 Black Friday **B-Stock** sale ran Nov 16–30 with discounts up to 67%. It was North
  America only and sold out within hours, and it included the MM-500. MM-500 B-Stock has sold for
  **$1,444 (Audio46)** and **$1,579 (Thomann)** against $1,699 new. The MM-520 is new, so B-Stock may be
  scarce. Sign up for Audeze's emails anyway. One open-box or B-Stock unit could cut **$100–$250** off
  the target (my estimate, based on MM-500 pricing).
- headphones.com offers 365-day returns, which lowers the risk of buying open-box there.
- **Backstop:** if the Dec 15 checkpoint below falls short, add a guaranteed source (seasonal shifts,
  selling unused gear) so the deadline doesn't depend on luck.

### Timeline and checkpoints

| Window | Track A | Track B / C |
|---|---|---|
| Oct 17–26 | Pick towns and categories, build one demo rescue site, write the scripts, set up a tracking sheet | Vet any candidates with the §7 checklist; spend ≤ $50 |
| Oct 27–Nov 23 | 10–15 owner contacts/week; **first paid job by about Nov 15** | Pitch businesses for any Track B names; watch Audeze B-Stock |
| **Nov 15 check** | Fewer than 30 qualified contacts or 0 replies → change the niche or script | |
| Nov 24–Dec 21 | Deliver jobs, ask every client for 2 referrals, do most outreach before the holidays | Black Friday window for Track C |
| **Dec 15 check** | **At least $900 banked?** If not, start the backstop now | Stop Track B spending if there are no inquiries |
| Dec 22–Jan 15 | New-year push (businesses plan their year); collect care-plan fees | |
| **Jan 15** | **Order the MM-520** (allow for preorder lead time) | |

### Scenarios (Track A only, estimates)

| Scenario | Jobs | Revenue |
|---|---|---|
| Conservative | 3 × $300 | $900 (needs Track C and the backstop) |
| Base | 5 × $400 | $2,000 (MM-520 funded) |
| Good | 7 × $450 + 3 care plans | $3,150+ |

---

## 7. Pre-purchase checklist (replaces the "5-minute drill")

Skip the domain if any of these fails.

1. **Status.** Check RDAP/WHOIS (lookup.icann.org). Is the domain actually deleted and available, or
   only in grace, redemption or pending delete? Who's the registrar, and can a sale be pushed within
   that registrar?
2. **Trademark and identity.** Search USPTO and the WIPO Global Brand Database, then Google the exact
   name. Was it a company, product, government body, school, charity, park or clinic? If so, skip it.
3. **Software footprint.** Search GitHub code, the Chrome Web Store, the Google Workspace Marketplace and
   npm/PyPI for the domain. If anything still calls it, skip it. **Never set up email or a catch-all on
   an expired domain.**
4. **Wayback, year by year.** Look at inner pages, not just the homepage, for CJK spam, casino, pharma,
   adult content or redirects to unrelated sites.
5. **Top 20 referring domains.** Are the links editorial, in context, on-topic and still live?
6. **Backlinks per referring domain.** Much above about 20:1 points to sitewide or syndicated links (my
   rule of thumb). Check the anchor-text spread too.
7. **Do the metrics agree?** DR, DA and TF should roughly line up. Keep TF/CF in a sensible range.
   Ignore SimilarWeb below 5k/month.
8. **Blacklists.** Google Safe Browsing (Transparency Report), Spamhaus DBL, SURBL, and the Wikipedia
   spam blacklist.
9. **Index.** Search `site:domain.com` and note what remains.
10. **The closeout question.** Write one sentence explaining why 10 days of public auction passed on it.
    If you can't, skip it.
11. **Buyer list.** Name 10 or more specific buyers who have a budget and a reason to want it. No list,
    no purchase.
12. **Net-out math.** Price minus commission, minus renewal (especially .io), minus escrow. Does it
    still beat 3 hours of Track A work?

---

## 8. Scripts

**Track A phone opener**

> Hi, is this the owner? I'm [name], I'm local. I was looking you up on Google Maps and the Website button
> on your listing goes to [a "domain for sale" page / an error page / a gambling site]. Customers who tap
> it may think you've closed. Fixing this is what I do. I can have a working site and the link corrected
> within 48 hours for $[X], and you own everything. Can I text you a screenshot of what customers are
> seeing?

**Track A email** (CAN-SPAM compliant)

> **Subject:** Your Google listing's website link is broken
>
> Hi [Name], the Website button on [Business]'s Google listing currently opens [describe; screenshot
> attached]. I set up replacement sites for local businesses: [package], live in 48 hours, $[X], all
> accounts in your name. Reply "yes" and I'll send a mockup.
>
> [Your name] · [postal address] · Reply "stop" and I won't email again.

**Track B email** (for a city + service domain)

> **Subject:** [CityService].com
>
> Hi [Name], I own [CityService].com and I'm offering it to a few [service] companies in [City] first.
> Exact-match names like this are easy for customers to remember from trucks, yard signs and radio ads.
> Buy now: $[price] ([checkout link]), or 3 monthly payments. Happy to answer questions.
>
> [Your name] · [postal address] · Reply "stop" to opt out.

---

## 9. Sources

**Target**
- MM-520 price and availability: [headphones.com](https://headphones.com/products/audeze-mm-520-headphones) · [TMR Audio](https://tmraudio.com/products/audeze-mm-520-planar-magnetic-headphones) · [Equipboard](https://equipboard.com/items/audeze-mm-520-studio-headphones)
- MM-520 launch: [headphones.com announcement](https://headphones.com/blogs/announcements/audeze-announces-the-release-of-mm-520-their-newest-open-back-studio-headphones) · [Sound on Sound](https://www.soundonsound.com/news/audeze-announce-mm-520) · [Stereonet](https://www.stereonet.com/news/audeze-mm-520-studio-headphones-boast-slam-tech) · [AV Watch (Japan launch)](https://av.watch.impress.co.jp/docs/news/2137251.html)
- B-Stock: [Head-Fi: Audeze 2023 Black Friday B-Stock](https://head-fi.org/threads/audezes-2023-black-friday-b-stock-sale.970565/) · [Audio46 MM-500 B-Stock](https://audio46.com/products/audeze-mm-500-planar-magnetic-headphone-b-stock) · [Thomann MM-500 B-Stock](https://www.thomannmusic.com/audeze_mm_500_b_stock.htm)

**Claims in the six methods**
- Drop-catching research: [USENIX Security 2017, "Game of Registrars"](https://usenix.org/conference/usenixsecurity17/technical-sessions/presentation/lauinger) · [WWW 2018, "Panning for gold.com"](https://www.securitee.org/files/dropcatching_www2018.pdf)
- Tools: [Karma.Domains overview (Ranktracker)](https://www.ranktracker.com/blog/finding-expired-domains-when-you-just-describe-what-you-want/) · [CatchDoms MCP (mcp.so)](https://mcp.so/servers/catchdoms-mcp?tab=tools) · [CatchDoms MCP (mcpservers.org)](https://mcpservers.org/de/servers/sambel/catchdoms-mcp)
- Wikipedia: [Pew Research, "When Online Content Disappears"](https://www.pewresearch.org/data-labs/2024/05/17/when-online-content-disappears/) · [Wikipedia Template:Usurped](https://en.wikipedia.org/wiki/Template:Usurped) · [Wikipedia Spam blacklist](https://en.wikipedia.org/wiki/Wikipedia:Spam_blacklist)
- Google: [Spam policies (expired domain abuse)](https://developers.google.com/search/docs/essentials/spam-policies) · [Homepage redirects treated as soft 404s (SER)](https://seroundtable.com/301-pages-home-soft-404-22811.html) · [GSQI soft-404 case study](https://www.gsqi.com/marketing-blog/redirects-less-relevant-pages-soft-404s/)
- Metrics: [Majestic Topical Trust Flow](https://blog.majestic.com/development/topicaltrustflow/) · [Moz Spam Score](https://moz.com/help/link-explorer/link-building/spam-score) · [SimilarWeb data accuracy](https://support.similarweb.com/hc/en-us/articles/360002219177) · [SimilarWeb "Not enough data"](https://support.similarweb.com/hc/en-us/articles/207698639-What-does-Not-enough-data-mean-)

**Market economics**
- Fees: [GoDaddy/Afternic commission](https://www.godaddy.com/help/what-is-list-for-sale-27761) · [Flippa fees (third-party, 2026)](https://exitbid.io/blog/flippa-fees-explained-2026)
- Sell-through and outbound: [Namecheap on sell-through rate](https://www.namecheap.com/blog/domain-name-sell-through-rate/) · [Appraise.net](https://appraise.net/blog/is-your-domain-portfolio-profitable) · [NamePros outbound conversion poll](https://www.namepros.com/threads/outbound-email-sales-conversion.1098417/)
- Expiry mechanics: [GoDaddy closeout pricing change](https://domainnamewire.com/2021/01/15/godaddy-to-change-closeout-auction-pricing/) · [Namecheap expiry/auction timeline](https://www.namecheap.com/blog/your-domain-expired/)
- Transfer lock: [ICANN Transfer Policy changes (WTR)](https://www.worldtrademarkreview.com/article/how-proposed-changes-icanns-transfer-policy-will-impact-domain-owners-and-registrants) · [Safenames](https://www.safenames.net/resources/blogs/safenames-blog/2025/04/16/how-will-proposed-changes-to-icann-s-transfer-policy-affect-domain-owners)
- .io: [Domain Incite, Chagos deal on ice (Apr 2026)](https://domainincite.com/31642-io-safe-for-now-as-trump-puts-chagos-deal-on-ice) · [.io renewal prices (domainoffer.net)](https://domainoffer.net/tld/io)

**Legal and security**
- [ACPA overview (The Fashion Law)](https://www.thefashionlaw.com/resource-center/anticybersquatting-consumer-protection-act/) · [Truffle Security Google OAuth research (The Hacker News)](https://thehackernews.com/2025/01/google-oauth-vulnerability-exposes.html) · [watchTowr expired-domain backdoors (The Hacker News)](https://thehackernews.com/2025/01/expired-domains-allowed-control-over.html)

**Track A signal**
- Google Business Profile websites shut down: [Bill Hartzer](https://www.billhartzer.com/local-search/google-shutting-down-business-site-websites/) · [Android Central](https://www.androidcentral.com/apps-software/google-shutting-down-business-profile-websites)

**Individual domains**
- [ConvertOnline Workspace add-on](https://workspace.google.com/marketplace/app/convertonline/153129029694) · [PodCurator Chrome extension](https://chromewebstore.google.com/detail/adhlnihfibfdaljhiibiehnnphafdmpm) · [ABATherapistJobs.com press release](https://techbullion.com/?p=659520) · [API Evangelist: Classif.io](https://providers.apievangelist.com/providers/classif-io/) · [FinLaw (Australia)](https://lawyersweekly.com.au/sme-law/27527-new-legal-locum-for-financial-services-launches)
- Heather's Choice: [Looper](https://www.looper.com/1488056/what-you-need-know-heathers-choice-from-shark-tank/) · [HuntTalk wind-down thread](https://www.hunttalk.com/threads/heathers-choice.331848/)
- Prusch: [Emma Prusch Farm Park (Wikipedia)](https://en.wikipedia.org/wiki/Emma_Prusch_Farm_Park) · [ABC7, 2024 dispute](https://abc7news.com/post/prusch-farm-park-foundation-san-jose-given-30-day-notice-vacate-emma/15495099/)
