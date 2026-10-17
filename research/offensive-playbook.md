# Offensive Playbook: Tech, Agents, Anonymous Assets

Prepared 2026-10-17. **What it has to fit:** a mysterious, high-end mixing persona. That means no
outreach, no content, no begging for work, and nothing tied to your name. Everything here runs under an
anonymous product or holding brand, and AI agents do the long, boring parts.

All dollar figures are estimates.

---

## 0. Ground rules

1. **Keep your name out of it.** Each play runs under its own brand (a product name, or a holding name for
   deals and shops). Your mixing name stays clean.
2. **Agents do the volume.** Scouting, translating, pricing, monitoring, drafting code, filing claims. You
   make the decisions and do the taste work.
3. **Push right up to the legal line, but not over it.** Crossing it costs you the asset: takedowns,
   payment-processor bans, lawsuits. §6 says exactly where the line is.

---

## 1. The other agent's two app ideas

Both point at real gaps, but neither is as empty as "a genuine niche gap" suggests.

| Idea | What already exists | The real gap |
|---|---|---|
| **System-wide crossfeed** | Crossfeed is a commodity inside DAWs (CanOpener Studio, Crossfire, ADAM's H200 utility, MathAudio). Sonarworks SoundID has a system-wide app, but it does calibration, not crossfeed. On Mac, SoundSource can host AU plugins system-wide. On Windows, Equalizer APO is free and loads VSTs. | No **simple, native, cheap, macOS Tahoe-ready** app does system-wide crossfeed or virtual speakers without stacking SoundSource plus a plugin. |
| **"DigiCheck NG, system-wide"** | RME's DigiCheck NG is **RME-hardware only**, so everyone else is locked out. **MiniMeters** ($20; Windows, macOS 14.2+, Linux) already captures system audio with spectrum, LUFS, stereometer, VU and oscilloscope. Its own page says it's still working on **output capture problems on macOS Tahoe**. | DigiCheck-grade *pro* analysis for any interface: bit statistics, a bit-perfect check, loudness logging, streaming-normalization preview, per-app metering. Plus Tahoe reliability. |

**Verdict: build them as one app, not two.** Both need the same core: capture system audio, process it,
play it back. So build one "headphone reference layer":
- crossfeed and virtual speakers
- analyzer
- streaming-normalization preview (hear Spotify/Apple/YouTube at -14/-16 LUFS)
- bit-perfect verifier (does your player actually send untouched bits to the DAC?)
- per-app metering (meter only Spotify, not your notifications)

That's one codebase and one launch, for engineers and Head-Fi audiophiles alike. It's also the app you'll
want on your own MM-520.

**How to build it fast (macOS first):**
- **Capture and DSP without a kernel driver:** Core Audio **process taps** (macOS 14.2+). Use
  `.mutedWhenTapped`, then route tap → aggregate device → your IOProc DSP → output.
  Open-source references for the call sequence: AudioTee and catap.
- **Crossfeed:** start from **libbs2b**, which distro packages list as MIT (confirm the upstream LICENSE),
  then add your own better algorithm on top.
- **Loudness:** BS.1770 / R128 loudness via **libebur128** (MIT). FFTs via Apple's Accelerate.
- **Windows later:** Equalizer APO already covers free system-wide DSP there. Mac is where the gap is.

**Business:**
- $29–$49 one-time, with an early-access price for Black Friday.
- Sell direct (Paddle or Lemon Squeezy) with a Developer ID-signed, notarized build.
- **Distribution with zero personal exposure:**
  - a KVR product listing
  - launch posts from the *product* account on Gearspace, Head-Fi and r/macapps
  - Product Hunt
  - Setapp later
- **"Sells forever" is wrong.** macOS audio changes break apps every year (see MiniMeters on Tahoe).
  Charge for major upgrades or offer an optional update plan.

**Revenue estimate:** launch revenue is unpredictable, from a few hundred to a few thousand dollars.
Treat January money from it as upside; the asset is worth more later.

---

## 2. Play: Japan → West gear arbitrage, scouted by an agent

**Why it works:** Yahoo! Auctions Japan and Mercari Japan are full of well-kept audio gear, listed in
Japanese, priced for a domestic market. **The language barrier is your edge.** An agent removes it in
seconds.

**The agent loop:**
1. Watch saved searches. Use the proxies' and marketplaces' *own* saved-search alerts as the data feed,
   not heavy scraping.
2. Translate each listing, and score condition from the photos and description.
3. Price against Reverb sold prices (Reverb Price Guide) for your resale market.
4. Compute the **landed cost**:
   - item price
   - proxy fee (Buyee, ZenMarket and FromJapan charge a per-item fee plus extras)
   - shipping inside Japan
   - international shipping
   - import duty and VAT
5. Alert only when landed cost is **≤ 65% of the median sold price**.

**What to buy:** compact, high-value items: drum machines, desktop synths and modules, pedals, rack
effects, mics, interfaces.
- Skip bulky keyboards, where shipping eats the margin.
- Japan runs on 100 V. Prefer gear with external or universal power supplies.

**Sell** under a shop name on Reverb, or to EU buyers while you're in Europe.

**Timing:** delivery takes 1–3 weeks and resale 1–4 weeks, so this is **the play most likely to produce
cash by mid-January** if you start now.

**Example math:** a $1,000 bankroll, 3–6 flips, $300–$1,200 profit.

---

## 3. Play: dead-software acquisition engine ("expired domains, but with real users")

**The thesis:** beloved plugins and Mac utilities die when their developers quit. Common signs: Intel-only,
no VST3/AAX/CLAP, broken on the latest macOS. Users keep asking "is X dead?". **Buy the IP and source
cheaply, revive it with AI-assisted porting, and relaunch it under your brand.**

**Discovery agent:**
- Crawl the KVR product database and forums for stale last-update dates and "discontinued" flags.
- Count unanswered "Apple Silicon version?" threads and Reddit/Gearspace mentions.
- Check Acquire.com and Flippa for app listings.
- Rank by demand signal ÷ estimated price.

**The deal:** an asset purchase with a written **IP assignment** covering code, name and trademark,
customer list where the law allows, and update rights. Offer $500–$5,000 upfront plus an optional
earn-out. Contact developers through the holding brand. It's B2B deal-making, not job hunting.

**The revival:**
- Port to JUCE or iPlug2, Apple Silicon native, with VST3/AU/AAX/CLAP builds.
- Add a system-wide mode using the §1 engine.
- Relaunch as "X 2". Past owners are your first buyers.

**Open-source variant:** forking abandoned **MIT/BSD** projects into a commercial product is legal. Forking
**GPL** is legal too, but you must publish your source. Add real value (UX, presets, platforms); otherwise
the community will call it out.

**Timing:** this is the biggest upside play. It pays after January.

---

## 4. Play: royalty sweep (money you've already earned)

**The agent's job:** assemble every credit you have from Discogs and MusicBrainz (both have open APIs),
streaming credits and liner notes. Map them to ISRC/ISWC codes, then check every registry:
- **SoundExchange:** US digital performance royalties. Producers, mixers and engineers get paid through a
  featured artist's **Letter of Direction**. It isn't retroactive unless the letter says so, and it pays
  you or a company you solely own.
- **The MLC and your PRO:** any writing or co-writing splits.
- **Neighboring-rights societies:** for any recording you *performed* on.

**Going forward:** make an LOD (points) a standard term on every mix, as top mixers do. Your persona makes
this easier, not harder. Scarcity is leverage.

**Timing:** payment cycles mean this probably won't help by January. Over a year, it's often the highest
return in this document, and it needs zero outreach beyond the clients you already have.

---

## 5. Play: niche-gap radar (stop guessing at gaps)

An agent that mines KVR, Gearspace, Head-Fi, Reddit and Mac-app communities for repeated "I wish there
were an app that…" requests and complaints.

**For each cluster it scores:**
- demand: mentions, upvotes, recency
- willingness to pay: people saying "I'd pay for…"
- existing solutions: it searches for them automatically, so a tool like MiniMeters would have been
  flagged before you wrote a line of code
- build effort

It feeds plays 1 and 3. Run it weekly. Pull data through official APIs or search APIs where possible.

---

## 6. Where the line actually is

| Fair game | Over the line (costs you the asset) |
|---|---|
| Clean-room re-implementation of any *feature* (DigiCheck-level analysis, crossfeed, metering). Functionality isn't copyrightable. | Copying code or UI artwork. Decompiling RME's app. Using **"DigiCheck"** or anything confusingly similar in your name or marketing (RME's trademark). |
| Commercial use of MIT/BSD code with attribution. Selling GPL software *with* source. | Shipping GPL code inside a closed app without publishing source. |
| Cross-border arbitrage. Regional sourcing. Pricing at what the market bears. | Undervaluing customs declarations (customs fraud). Grey-market or region-locked license keys. |
| Official APIs, saved-search email alerts, polite low-volume collection of public data. | Scraping in breach of terms at volume, or harvesting personal data (GDPR exposure in the EU). |
| Launch posts from the product account, open about who made it. | Fake reviews or sock-puppet accounts. These get products banned from KVR, Reddit and payment processors fast. |
| Buying abandoned IP with a written assignment. | Reviving closed-source abandonware *without* buying the rights. |

---

## 7. The next 90 days

| When | Do |
|---|---|
| **This week** | Start the Japan arbitrage agent (saved searches, landed-cost calculator, alerts). Start the royalty sweep (credit export). Spec the merged headphone-reference app. |
| **Weeks 2–5** | First 2–4 arbitrage buys. App MVP: process tap + crossfeed + LUFS/true-peak + normalization preview, on macOS 14.2–Tahoe. Gap radar v1. |
| **Late Nov** | Early-access launch for Black Friday (KVR listing, product-account posts, Product Hunt). Resell the first arbitrage items. |
| **Dec–Jan** | Arbitrage round 2. App updates from user feedback. Top 5 acquisition targets from the radar, first offers sent through the holding brand. |
| **Jan, Stockholm** | Buy the MM-520 by the Thomann VAT-refund route (`extra-income-sprint.md` §3). |

**Cash by mid-January:** arbitrage first, then early-access app sales, then any unused licenses you sell
anonymously on forums. Your $450/month savings stay the backstop. Everything else in this playbook builds
assets that pay after January.
