# Lantern Camp — Meta Ads & GTM Integration Data Sheet

**Last edited:** 2026-06-24 by Claude

This document tracks configuration, technical architecture, and verification steps for the Meta Ads and Google Tag Manager (GTM) tracking pipeline on the Lantern Camp website and Mews booking engine.

---

## 1. Accounts & Infrastructure Reference

### Meta & Assets
*   **Business Manager (Client):** `"Addison's Businesses"` (Owned by Addison Godine)
*   **Business Manager (Freelancer):** `"Ben"` — Portfolio ID: `158371295154274`
*   **Facebook Page:** `"Lantern Camp"` — Page ID: `1201507149693060` (Full control via partner access)
*   **Ad Account:** `"Lantern Camp"` — ID: `1585226713251054` (Full control)
*   **Instagram:** `@staylanterncamp` — ID: `17841472888180622` (Content/Ads access)
*   **Active Meta Pixel:** `"Lantern Camp Pixel"` — ID: `2012732439334242` (Installed natively on Squarespace via Marketing → Meta Pixel & Ads; confirmed firing PageView and Purchase)
*   **Duplicate Unused Pixels:**
    *   `"Lantern Camp Pixel"` — ID: `1312404570473413`
    *   Two `"Ben's Pixel"` instances
    *   *Note:* These are not in use; cleanup is optional.

### Google Ads & Assets
*   **Ad Account:** `"Lantern Camp"` — ID: `932-585-7656` (Full control)
*   **Conversion Action:** `"Mews Purchase"` (Website conversion action for booking checkout)
*   **Conversion ID:** `18268127910`
*   **Conversion Label:** `N3p7CPXn58QCeKaF9oZE`
*   **Enhanced Conversions:** Configured via GTM User-Provided Data
*   **Developer Token:** `FFMKBX7IZyJ1sexxJJPUxw` (Basic Access, Swardy Manager)

### Funding & Daily Guardrails
| Metric | Detail |
| :--- | :--- |
| **Payment Method** | MasterCard ending `9744` (Active on ad account) |
| **Account Spending Limit** | $500 (Set for June, resets 1st of month) |
| **Meta Automatic Limit** | $50/day (Meta-set velocity cap for new accounts) |

### Web & Property Systems
*   **CMS Website:** `lanterncamp.com` (Squarespace, Core Plan)
*   **Booking Platform:** `app.mews.com` (Mews Property Management System; bookings occur on this separate domain)
*   **Mews Distributor ID:** `0d537195-8d8c-4976-9261-b41d00cb6bab`
*   **Google Analytics 4 ID:** `G-3JJ6S8J5ZD`
*   **Google Tag Manager Container ID:** `GTM-NBWD4J5P` (Installed on Squarespace via Code Injection; published. Also entered in Mews under *Booking engine* → *Default* → *Google Tag Manager ID*)
*   **Mews GTM Integration Settings:** `"Collect emails for bookings in GTM purchase event"` enabled in Mews.
*   **Mews dataLayer Payload:** Mews fires a clean GA4-style ecommerce dataLayer. The Purchase event contains:
    ```javascript
    {
      event: 'purchase',
      ecommerce: {
        value: 239.00, // Net value
        currency: 'USD',
        transaction_id: '...',
        // ...
      },
      grossValue: 260.51,
      reservationOwnerEmail: 'guest@example.com'
    }
    ```

---

## 2. Technical GTM Architecture

### Variables Confirmed Active
*   **`DLV - Booking Value`** → maps to `ecommerce.value` (Pulls net value, excluding tax)
*   **`DLV - Currency`** → maps to `ecommerce.currency` (Pulls USD)
*   **`DLV - Reservation Email`** → maps to `ecommerce.reservationOwnerEmail` (Pulls raw email string for Advanced Matching)

### Configured Tags & Triggers
*   **Tag 1: Meta Pixel - Purchase**
    *   *Type:* Custom HTML
    *   *Trigger:* Custom Event → `Mews Purchase`
    *   *Setup Tag:* Meta Pixel - All Pages (fires first to initialize pixel)
    *   *Script:* Manually scrapes `window.dataLayer` at runtime to read `ecommerce.value`, `ecommerce.currency`, and `reservationOwnerEmail`, then calls `fbq('track', 'Purchase', {value, currency})`. Also passes email to `fbq('init')` for Advanced Matching before the purchase call.
    *   *Note:* GTM variable substitution (`{{DLV - Booking Value}}`) caused timing issues — the variable resolved as undefined at tag fire time. Direct `window.dataLayer` scraping bypasses this.
*   **Tag 2: Meta Pixel - All Pages**
    *   *Type:* Meta Pixel Community Template (Base Pixel)
    *   *Pixel ID:* `2012732439334242`
    *   *Event:* Standard Page View
    *   *Trigger:* All Pages / Page View
    *   *Note:* Added during troubleshooting; this tag is what successfully loads the pixel base code on the Mews domain (`app.mews.com`).
*   **Tag 3: Google Ads - Purchase Conversion**
    *   *Type:* Google Ads Conversion Tracking
    *   *Trigger:* Custom Event → `Mews Purchase`
    *   *Settings:*
        *   **Conversion ID:** `18268127910`
        *   **Conversion Label:** `N3p7CPXn58QCeKaF9oZE`
        *   **Conversion Value:** `{{DLV - Booking Value}}`
        *   **Currency Code:** `{{DLV - Currency}}`
        *   **Transaction ID:** `{{DLV - Transaction ID}}`
        *   **Provide User-Provided Data:** Checked → select `{{User-Provided Data}}` (Variable 1)
*   **Tag 4: Google tag - Google Ads (Base Config)**
    *   *Type:* Google tag
    *   *Tag ID:* `AW-18268127910`
    *   *Trigger:* Initialization - All Pages
*   **Tag 5: Conversion Linker**
    *   *Type:* Conversion Linker
    *   *Trigger:* All Pages
    *   *Settings:* Enable linking across domains (defaults to true)

---

## 3. Campaign Configuration & Launch Status

> **Account status as of June 24, 2026: ACTIVE. Billing resolved. One active traffic campaign (Drive Market).**

### Campaign 1: HOMEPAGE traffic - static single image
*   **Status:** PAUSED (June 21) — replaced by Campaign 1b (Drive Market) below
*   **Objective:** Traffic / Landing Page Views
*   **Creative:** Single static image (Field Cabin 1 photo)
*   **Destination:** lanterncamp.com homepage
*   **UTM Link:** `http://www.lanterncamp.com?utm_source=instagram&utm_medium=ad&utm_campaign=single+static&utm_id=homepage+single+static+1`
*   **GA4 attribution:** shows as `instagram / ad`
*   **Performance (lifetime):** $179.56 spend, 19,494 impressions, 1,987 clicks, ~996 landing page views @ $0.18 CPV
*   **Why paused:** Two problems found in the data. (1) **Geo waste** — ~72% of spend went to Maine *locals* (home market, not drive-market guests). (2) **Creative starvation** — Meta picked one of the two creatives within days and starved the other (~$62 vs ~$1–2 spend), so the second never got a fair test. Also skewed heavily **65+** (53% of spend). Replaced with corrected geo + age targeting in Campaign 1b.

### Campaign 1b: HOMEPAGE traffic - static single image - [Drive Market]
*   **Status:** ACTIVE (launched June 21), $50/day
*   **Objective:** Traffic / Landing Page Views
*   **Creative:** `field cabin 2 - 4:5 - traffic`. **Only ONE ad carried over** from the duplication — the intended second creative did not copy. Campaign was duplicated with a 50/50 creative split planned for the first 7 days; re-add the second creative in Ads Manager if running that A/B test.
*   **Destination:** lanterncamp.com homepage
*   **Geo targeting:** MA, CT, NY, NJ, NH, VT, RI, PA, MD, DC, FL + Cumberland County ME (Portland). **York County ME could NOT be added** — Meta rejected it as a targetable region. Workaround if ever needed: add York-area cities individually (Kittery, York, Ogunquit, Wells, Kennebunk, Biddeford, Saco). Decided not worth it — that border area is largely covered by the NH/MA targeting.
*   **Saved audience:** "Drive States + Cumberland County" — refined June 23 with **age min 25** + interests **Ecotourism, hiking trails, Acadia National Park, Maine, Glamping**. Wanted an age *max* of 54, but **Advantage+ audience only allows a minimum-age floor, not a cap**; a hard 25–54 cap requires switching to original audience options (not done — kept Advantage+ for optimization). Age/interest edits are made in the **saved audience** section, not the ad set editor.
*   **UTM Link:** `https://www.lanterncamp.com?utm_source=Instagram&utm_medium=feed%2C+static&utm_campaign=drive_market_traffic&utm_id=HOMEPAGE+traffic+-+static+single+image+-+%5BDrive+Market%5D`
*   **GA4 attribution:** shows as `Instagram / feed, static` — **note the capital I** (case-sensitive in GA4; differs from Campaign 1's lowercase `instagram`).
*   **Age performance (June 24, ~$134 spend, ~948 LPVs):** 65+ keeps climbing (~27% of spend) but is among the *cheapest* CPV (~$0.13). Without switching off Advantage+ it will keep gravitating older. **Open hypothesis (UNPROVEN — do not build strategy on it):** for a traffic campaign, older viewers may still have value if they share the property with adult kids/grandkids. No data supports this.
*   **Geo performance (June 24):** Maine down from 72% (old campaign) to ~16% — all Cumberland County, intentional. NY ~22%, MA ~20%, FL ~13%. Targeting fix is working.

### Campaign 2: Michelle Lawrence Partner Conversion (Sales)
*   **Status:** PAUSED (turned off June 19 — 0 attributed purchases, not viable until conversion data proves out)
*   **Objective:** Sales / Conversions
*   **Creative:** Michelle Lawrence partnership carousel post
*   **Destination:** Mews booking page (direct — bypasses lanterncamp.com)
*   **UTM Link:** `https://app.mews.com/distributor/0d537195-8d8c-4976-9261-b41d00cb6bab?utm_source=instagram&utm_medium=carousel&utm_campaign=ml-collab&utm_id=ml-collab-2`
*   **GA4 attribution:** shows as `instagram / carousel`
*   **Performance (lifetime as of June 19):** $38.70 spend, 3,303 impressions, 135 clicks, 0 reported purchases
*   **Conversion Event:** Purchase
*   **Note:** Both campaigns use `utm_source=instagram` — GA4 traffic will appear under instagram rows, not facebook/cpc
*   **Dashboard filter tip:** To isolate paid Meta traffic in Panel 4 (traffic table), filter by `utm_medium contains ad OR carousel` — do NOT filter by source/facebook, as paid sessions show as instagram not facebook.

### Campaign 3: HOMEPAGE conversion - static single image
*   **Status:** PAUSED (abandoned)
*   **Objective:** Sales / Conversions
*   **Performance:** $0 spend, no impressions
*   **Note:** Abandoned — strategy is traffic-first until conversion data is proven. Michelle Lawrence (Sales) also showing 0 attributed purchases, confirming conversion campaigns aren't viable yet. Can delete or leave paused.

---

## 4. Problems Solved & Milestones Achieved

*   **Fixed Core Pixel Script Void:** Resolved the core bottleneck where browser developer consoles generated an `Uncaught ReferenceError: fbq is not defined` error on the Mews checkout screens. The newly deployed "Meta Pixel - All Pages" base tag successfully loads the core tracking utility across the checkout flow.
*   **Verified Data Delivery Sequence:** Confirmed via GTM debugging frames and Pixel Helper that the `Meta Pixel - Purchase` tag and base Pixel successfully fire on the Mews confirmation page.
*   **Established Multi-Domain Footprint:** Meta Events Manager has officially updated its native "Websites" dataset directory to register traffic arriving from `app.mews.com` and shows Purchase events arriving.
*   **Container Deployed to Production:** Published GTM container setup live.

---

## 5. ⚠️ Open & Deferred Issues (Known)

> [!NOTE]
> ### Issue 1: Purchase Value & Currency Passing to Meta ✅ RESOLVED (June 19, 2026)
> *   **Root Cause:** GTM variable substitution (`{{DLV - Booking Value}}`) in the tag HTML resolves at compile time before the Mews dataLayer push is available, causing values to be undefined at fire time. The Meta Pixel community template's Object Properties approach also failed to pass values through.
> *   **Fix:** Replaced the Meta Pixel - Purchase tag with a **Custom HTML tag** that scrapes `window.dataLayer` directly at runtime (iterating backwards through the array to find `ecommerce.value`, `ecommerce.currency`, and `reservationOwnerEmail`), then calls `fbq('track', 'Purchase', {value, currency})` and passes email to `fbq('init')` for Advanced Matching.
> *   **Verified:** Facebook Pixel Helper confirmed Purchase event with `currency: USD` and `value: 229` on the Mews confirmation page. ✅

> [!NOTE]
> ### Finding: Purchase tracking — where to look (June 24, 2026)
> The pixel **is** recording Purchase events. The confusion came from querying the wrong scope:
> *   **Campaign-attribution field** (`actions:omni_purchase` at campaign level) returns **"Not available"** for our traffic campaign. This is expected — a *traffic* objective isn't optimized for conversions and won't be credited purchases. It is NOT evidence that no purchases happened.
> *   **Raw dataset event log** (Events Manager → dataset, or MCP `ads_get_dataset_stats` with `aggregation: event`) shows the **actual Purchase events** the pixel fired, from any visitor, regardless of ad attribution. This is the source of truth for "did the pixel fire."
> *   So far, the Purchase events in the log are all **test bookings** (clustered, ~12 total — see Phase 1 cleanup), not real guests.

> [!NOTE]
> ### Finding: Reporting timezone is Pacific, not Eastern (June 24, 2026)
> The pixel/ad-account reporting clock is **Pacific (-0700)**, while the property and bookings are **Eastern**. A late-night Maine booking lands on the *previous calendar day* in Meta's views. Example: a Mews booking created **1:21 AM EDT June 19** appears under **June 18** in Events Manager (10:21 PM PDT). This explains an earlier "missing" purchase that was actually present, just filed a day earlier. **Not changed** — decided not worth adjusting for now; just be aware when matching Mews dates to Meta dates.

> [!WARNING]
> ### Finding: Event deduplication at 66% — server events from an unconfirmed source (June 24, 2026)
> Events Manager shows **Total event coverage 66%** (Meta recommends 75%+). The dataset is receiving both **browser events (~433/7d)** and **server events (~498/7d)**, but they aren't consistently sharing an Event ID, so Meta can't cleanly dedupe them.
> *   **Browser events:** the GTM Custom HTML pixel tag (expected).
> *   **Server events — source UNCONFIRMED.** Leading hypothesis: **Squarespace's native Meta integration** (Marketing → Meta Pixel & Ads), which sends server-side/CAPI events on its own. It is **NOT Mews** — per [Mews docs](https://docs.mews.com/booking-engine-guide/integrations/google-tag-manager), Mews only integrates via GTM (browser-side), with no native Meta CAPI.
> *   **Important implication:** if the server feed is Squarespace, it covers **lanterncamp.com only — not app.mews.com**. So a *Purchase* on the Mews confirmation page has **no server-side backup**; it relies entirely on the browser tag firing. If that tag is blocked (e.g. in-app browser), the purchase is lost with no fallback.
> *   **To confirm:** check Squarespace → Marketing → Meta Pixel & Ads (is it toggled on?), and inspect whether the dataset's server PageViews originate from lanterncamp.com.

> [!NOTE]
> ### Finding: Traffic campaigns cannot attribute purchases here (June 24, 2026)
> We cannot tie a booking back to the traffic ad — not even view-through ("was this buyer shown the ad") — and cannot reverse-engineer it manually. Why:
> *   The traffic ad lands on **lanterncamp.com**; the booking happens on **app.mews.com** (different domain). The click identifier (`fbclid`/`fbc` cookie) is domain-specific and does **not** survive the hop, so click-through matching is broken before checkout.
> *   The only bridge that survives is the **hashed email** (Advanced Matching) we send on the Purchase event — Meta can match the buyer to an ad exposure that way, but a traffic objective barely populates this and it's unreliable.
> *   Meta never exposes individual-level "email X saw ad Y," so there's nothing to cross-reference manually.
> *   **Takeaway:** to actually prove ad→booking attribution, run a **Purchase-optimized conversion campaign** (Meta builds and reports the linkage natively). The traffic campaign will never yield it, no matter how the data is sliced.

> [!WARNING]
> ### Issue 2: Value-less Purchase Conversion Counting Unverified
> *   **Problem:** Whether Meta registers a value-less Purchase event as a countable conversion has not yet been confirmed (Test Events check was deferred).
> *   **Verification Action:** After going live, watch the ad set's Results column in Ads Manager on the first real booking.
>     *   If the Results column increments by one Purchase, volume optimization is validated.
>     *   If a booking occurs and Results does not update, the campaign must be paused and the tracking fixed immediately.

> [!NOTE]
> ### Flag 3: Mews Content Consent Constraints (`advertising: false`)
> The raw dataLayer logs captured on the confirmation screen explicitly outputted `"advertising": false` inside the `trackingConsents` object block. Because testing was completed within a sandbox GTM preview window, the browser bypassed this gate. It remains untested whether real live guest transactions will be dropped or suppressed by Mews if this structural flag defaults to false.

> [!WARNING]
> ### Issue 4: MEWS Booking Email Notifications Silent (In Progress)
> *   **Problem:** MEWS is not sending email notifications when bookings are processed.
> *   **Troubleshooting Action:** Attempted to configure native notifications within MEWS (unsuccessful due to access restrictions). Want to troubleshoot why notifications are silent, check if Zapier integrations can be set up, and obtain backend API access to make changes to MEWS.
> *   **Resolution Status:** Addison connected Ben with MEWS account manager (William) on June 16. Ben followed up, and followed up again on June 22, and Addison also bumped the thread; still waiting for a response.

---

## 6. Immediate Verification & Cleanup Roadmap

### Phase 1: Mews Backend Housekeeping & Cleanup
*   [x] **Inventory Release:** Cancelled **all test reservations (~12 total, across two clusters made days apart** — an initial ~5 including #17/#18/#19/#22, then a second wave of ~7) inside the Mews operations portal to restore cabin inventory. (Refunded fully; no fees.) *Note:* these test bookings are the source of the clustered Purchase events visible in the pixel dataset log (see Section 5, "Purchase tracking — where to look") — they are NOT real guest revenue.
*   [ ] **Correct System Typo:** Navigate to the Mews Property Configuration settings. Locate the `item_category3` text attribute and change the entry from `"Orlando, FL"` to `"Orland, Maine"` to prevent skewed regional dimensions in upstream Google Analytics data arrays.
*   [ ] **(Optional) Remove Duplicate Pixels:** Remove duplicate unused pixels in the Meta account (another "Lantern Camp Pixel" ID `1312404570473413` and two "Ben's Pixel" instances).

### Phase 2: Live Guest Checkout & Campaign Launch Audit
*   [ ] **Results Column Watch:** Monitor the Ads Manager Results column on the first real booking post-launch. Confirm it increments by 1 Purchase.
*   [ ] **Meta Event Verification:** Open Meta Events Manager, scale the calendar date selector to include Today, navigate to the Purchase tracking summary, and click View Details.
*   [ ] **Metadata Structural Check:** Inspect the parameters log to check whether value and currency parameters are being captured (expecting them to be missing/value-less for now, but need to check for any unexpected errors).
*   [ ] **Ads Manager Column Customization:** Customize the primary Ads Manager dashboard view to display Purchases, Purchase Conversion Value, and Purchase ROAS metrics side-by-side (preparing the layout for when the value parameters are fixed).

---

## 7. Decision Log

*   **Booking Volume Optimization (June 13, 2026):** Chose to launch the campaign optimizing for booking **volume** (maximizing conversions) now, deferring the value/currency parameter fix and revenue/ROAS reporting to a later session. Campaign safety is bounded by the $500 Account Spending Limit (ASL) and $50/day limit.

---

## 9. 📊 Unified Tracking Dashboard — Implementation Plan

### Goal
A single Looker Studio page that answers: "Is our marketing effort producing bookings, and from where?"

**Audience:** Addison (primary), Ben (secondary). Glanceable summary with links to native tools (Ads Manager, Events Manager, GA4) for deeper dives.

**Source of truth:** Mews (all bookings land here regardless of channel). Meta Ads and GA4 are estimators / pipes — they inform the dashboard but do not define revenue.

---

### Dashboard Layout (4 Panels)

| Panel | Question | Source |
| :--- | :--- | :--- |
| **1 — Mews Ledger** | How many bookings? What revenue, by channel? | Mews CSV → Google Sheet |
| **2 — Meta Performance** | What did we spend? What did Meta report? | Meta Ads (Windsor.ai connector) |
| **3 — Connection Layer** | Is the pipe working? Sessions from Meta vs. Mews direct bookings (side-by-side, not merged) | GA4 + Panel 1 |
| **4 — Traffic Overview** | Where does site traffic come from? | GA4 |

**Channels tracked in Panel 1:**
- Direct (Mews native bookings) — full revenue
- Airbnb — gross minus ~15% OTA fee = net
- Booking.com — gross minus ~17% OTA fee = net
*Note: Numbers will not tie across panels by design — different attribution windows, consent gaps, modeled conversions. Mews is truth; others are directional.*

---

### Build Phases (in order)

#### Phase 0 — Pre-work (answer these before building)
- [ ] **OTA revenue question:** Airbnb/Booking.com bookings *do* block the Mews calendar (channel manager confirmed in use, set up by third-party company). Open question: do those reservations appear in Mews with a dollar amount attached, or just as calendar holds? Check Mews Reservations list for any past OTA booking and look for a revenue figure. **This determines whether the Google Sheet is self-populating from Mews exports or requires manual OTA revenue entry.**
- [x] **Meta Ads data source:** ~~Windsor.ai~~ NOT needed. Meta MCP can pull campaign performance data directly; we dump it to a Google Sheet on a cadence, and Looker Studio connects to that Sheet natively (free). Slightly manual but no extra vendor accounts needed. Ben or I can run the pull anytime in the MCP session.

#### Phase 1 — GA4 Cross-Domain Fix ✅ COMPLETE (June 17, 2026)
- [x] In GA4: Admin → Data Streams → web stream → Configure tag settings → "Configure your domains" → added `lanterncamp.com` and `app.mews.com`
- [x] In GTM: Created new **Google Tag** (`GA4 Configuration - Mews Cross Domain`) with Measurement ID `G-3JJ6S8J5ZD`, trigger scoped to Mews hostname only (`Page Hostname contains mews.com`) to avoid double-tracking on Squarespace where native GA4 already runs. Published as version "GA4 cross-domain linker - Mews".
- [x] Verified: `/distributor/0d537195-8d8c-4976-9261-b41d00cb6bab` (Mews) appeared in the same GA4 Realtime session as lanterncamp.com pages. Cross-domain stitch confirmed working.

#### Phase 2 — Mews Google Sheet (truth anchor)
- [ ] Export Mews bookings report as CSV (monthly or since launch)
- [ ] Paste into a Google Sheet with columns: `Date | Channel | Nights | Gross Revenue | OTA Fee % | Net Revenue | Guest Email (optional)`
- [ ] Add formula column: `Net Revenue = Gross * (1 - OTA Fee %)`
- [ ] Decide update cadence: weekly manual export to start; Mews API or Zapier later if volume warrants it
- [ ] For Airbnb/Booking.com bookings not in Mews: add manually from OTA host dashboards until channel manager sync is confirmed

#### Phase 3 — Looker Studio Shell
- [ ] Create new Looker Studio report at [lookerstudio.google.com](https://lookerstudio.google.com)
- [ ] Add Data Source 1: Google Analytics 4 (native free connector → property G-3JJ6S8J5ZD)
- [ ] Add Data Source 2: Google Sheets (connect to the Mews bookings sheet from Phase 2)
- [ ] Add Data Source 3: Meta Ads performance sheet (populated via Meta MCP pull → Google Sheet; connect same way as Mews sheet)

#### Phase 4 — Build the 4 Panels
- [ ] **Panel 1 (Mews Ledger):** Scorecard: total bookings, total net revenue. Bar chart: net revenue by channel. Date filter. Source: Google Sheet.
- [ ] **Panel 2 (Meta Performance):** Table by campaign: Spend | Impressions | Clicks | CTR | Reported Conversions | Cost per Result. Source: Windsor.ai/Meta.
- [ ] **Panel 3 (Connection Layer):** Two scorecards side-by-side — "GA4 Sessions from Facebook/CPC" and "Mews Direct Bookings." Not merged, just directional comparison. Source: GA4 + Sheet.
- [ ] **Panel 4 (Traffic Overview):** Sessions by source/medium (bar). Top landing pages (table). Source: GA4.
- [ ] Add a last-updated timestamp and links to: Ads Manager, Events Manager, GA4, Mews
- [ ] Share with Addison (view access)

#### Phase 4b — Dashboard Data Automation (future)
- [ ] **Meta performance:** Once billing resolved, pull via Meta MCP in-session weekly. Can schedule automated weekly pull via Claude scheduled tasks if needed.
- [ ] **Mews bookings sheet:** Manual export + paste for now (5 min/week). Long-term: automate via Zapier (Mews → Google Sheets). Blocked until Addison grants Ben "account manager" role in Mews (see Issue 4).
- [ ] **GA4 + Search Console:** Already live/automatic — no maintenance needed.
- *Note:* Zapier integration is in progress but blocked on Mews permissions — separate workstream.

#### Phase 5 — OTA Email Matching (do after ~10+ bookings)
- [ ] Export guest email list from Mews (direct bookings only — see caveat below)
- [ ] In Meta: Audiences → Create Audience → Customer List → upload email column
- [ ] Meta hashes and matches against users exposed to your ads
- [ ] Gives directional signal: what % of direct guests were also reached by Meta campaigns
- *Caveat: Airbnb passes a masked proxy email (e.g. `abc123@guest.airbnb.com`) to SiteMinder/Mews, not the guest's real email. Meta cannot match these. Email matching only works for direct Mews bookings where the real guest email is captured. Booking.com behavior likely similar — verify.*

#### Phase 6b — Google Search Console ✅ Partially Complete (June 17, 2026)
- [x] Verified lanterncamp.com in Search Console under bswardlick@gmail.com (verified automatically via GA4 account link)
- [x] Search Console already linked to GA4 since May 4, 2026 by hundllc@gmail.com — data has been collecting since then
- [ ] ~~Ask Addison to add Ben as a user on hundllc's Search Console property~~ — not pursuing historical data
- [ ] Once data populates (allow a few days for new property), add Search Console as a data source in Looker Studio
- [ ] Add a "Top search queries" table to Panel 4 alongside the traffic overview
- *Note:* New property (bswardlick@gmail.com) currently shows "processing data" — normal for a new verification. Historical data is under hundllc@gmail.com's property.

#### Phase 6c — Fix Value/Currency Passing ✅ RESOLVED (June 19, 2026)
- [x] Replaced Meta Pixel - Purchase tag with Custom HTML that scrapes `window.dataLayer` directly
- [x] Verified: Facebook Pixel Helper shows `currency: USD`, `value: 229` on Mews confirmation page
- [ ] Once billing resolved: enable Meta ROAS column in Ads Manager and add Purchase Conversion Value to Panel 2
- [ ] Consider Meta Conversions API (offline events) to send Mews bookings back to Meta for fuller attribution — useful when volume warrants it

---

### Open Questions (blocking or shaping)
1. ~~Do OTA bookings appear in Mews with a revenue figure?~~ **Resolved.** Confirmed: Airbnb bookings sync into Mews via **SiteMinder** (the channel manager / third-party setup company) with full gross revenue, nightly rate, reservation source label ("AirBnB"), and a travel agency confirmation number. Mews export is the single source for all booking revenue across channels. Filter by `Reservation source` to break out Direct vs. Airbnb vs. Booking.com.
2. ~~Windsor.ai~~ Resolved — not needed; using Meta MCP → Google Sheet instead.
3. ~~Channel manager unknown~~ **Resolved.** Channel manager is **SiteMinder**. Set up by the third-party company that originally configured Mews.

---

### Current Status
- **Phase 0:** ✅ Complete — all questions resolved
- **Phase 1:** ✅ Complete — GA4 cross-domain verified working (June 17, 2026)
- **Phase 2:** ✅ Complete — Mews bookings sheet live in Looker Studio (12 bookings, $5,318 net revenue)
- **Phase 3:** ✅ Complete — Looker Studio report created ("Lantern Camp Performance Dashboard"), GA4 + Mews Sheet + Meta Sheet all connected
- **Phase 4 (Panels):** 🔄 In progress
  - Panel 1 (Mews Ledger): ✅ Done — Total Bookings, Total Net Revenue, Net Revenue by Channel (Airbnb vs Direct)
  - Panel 2 (Meta Performance): ✅ Stubbed — table live with manual data; needs refresh post-billing (see blockers below)
  - Panel 3 (Connection Layer): ⬜ Not started
  - Panel 4 (Traffic Overview): ⬜ Not started
- **Phase 5–6:** Deferred

---

### 🎯 Top Priority Next Session
Add ROAS scorecard to Panel 2 in the Looker Studio dashboard now that value/currency is passing (Issue 1 resolved). ROAS = Purchase Conversion Value ÷ Spend. Requires Meta billing to be resolved first so campaigns are active and accumulating real conversion data.

---

### 🔴 Blockers & Post-Billing To-Dos

> **STATUS:** ✅ Billing resolved. Account ACTIVE as of June 24. Drive Market traffic campaign running live at $50/day.

- [x] **Fix Meta billing & suspension** — Resolved.
- [ ] **Refresh Panel 2 data via MCP** — Pull live campaign metrics via Meta MCP and overwrite the manual stub data in the Meta Performance tab of `Lantern Camp Dashboard Data` Google Sheet. Now unblocked.
- [ ] **Add Campaign Type column to Panel 2** — Column exists in the local Excel file but Looker Studio isn't reflecting it yet. After MCP refresh, confirm `Campaign Type` (Traffic / Sales) appears as a dimension in the table.
- [ ] **Rename Conversions → Results in Looker Studio** — The metric label in the Panel 2 table should read "Results" not "Conversions" to avoid confusion with actual purchases. Update after sheet refresh.
- [ ] **Resume campaign delivery** — Confirm campaigns re-activate automatically once account is unsuspended; if not, manually unpause in Ads Manager.

*Last updated: June 24, 2026*

---

## 8. 🔑 Partnership Ad Codes / Promotion Codes

*   **Michelle Lawrence Collab Post (June 12):**
    *   *Ad Code:* `adcode-Q9jTBBX3MltygKh25ZgBuxJxXEeyUtItDbqs77jKHndCBSE_3X9hmtWVpGaUrueZGBU`
    *   *Note:* Set up on her backend. This allows us to boost/promote the Collab Post directly from our Meta Ads Manager.
