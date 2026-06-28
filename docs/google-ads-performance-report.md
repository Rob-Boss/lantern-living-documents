# 📊 Lantern Camp Unified Performance Report
**Date Range:** Last 30 Days (Including Today) | **Generated At:** `2026-06-28 12:58:35`

This report compiles performance data across Google Ads, Meta Ads (manually input), and Google Analytics 4 (GA4) traffic analytics to provide a single view of our marketing funnel.

## 📈 Executive Summary
| Channel | Spend | Impressions | Clicks | CTR | GA4 Sessions | GA4 Users | Conversions |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Google Ads (Search)** | $42.57 | 278 | 39 | 14.03% | 43 | 36 | 0 |
| **Meta Ads (Paid)** | $487.67 | 58,152 | 4,047 | 6.96% | 2,414 | 2,323 | 0 |
| **Total Paid Channels** | **$530.24** | **58,430** | **4,086** | **6.99%** | **2,457** | **2,359** | **0** |

## 🔍 Google Ads Performance
| Campaign Name | Status | Spend | Clicks | Impressions | CTR | Conversions | Cost/Conv |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Lantern Camp - Search - bottom_of_funnel | `ENABLED` | $10.78 | 11 | 36 | 30.56% | 0 | - |
| Lantern Camp - Search - mid_funnel | `ENABLED` | $31.79 | 28 | 242 | 11.57% | 0 | - |

## 👥 Meta Ads Performance
| Campaign Name | Status | Spend | Clicks | Impressions | CTR | Conversions | Cost/Conv |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| Campaign 1b: HOMEPAGE traffic - static single image - [Drive Market] | `ACTIVE` | $269.41 | 1,925 | 35,355 | 5.44% | 0 | - |
| Campaign 2: Michelle Lawrence Partner Conversion (Sales) | `PAUSED` | $38.70 | 135 | 3,303 | 4.09% | 0 | - |
| Campaign 1: HOMEPAGE traffic - static single image | `PAUSED` | $179.56 | 1,987 | 19,494 | 10.19% | 0 | - |

### Meta Ads Demographics Breakdown (Age)
| Age Group | Impressions | Landing Page Views | Spend | % of Spend | Cost per LPV |
| :--- | :---: | :---: | :---: | :---: | :---: |
| 25–34 | 4,172 | 138 | $21.53 | 8.0% | $0.16 |
| 35–44 | 6,627 | 283 | $40.16 | 14.9% | $0.14 |
| 45–54 | 6,992 | 334 | $51.50 | 19.1% | $0.15 |
| 55–64 | 6,570 | 447 | $54.31 | 20.2% | $0.12 |
| 65+ | 10,746 | 716 | $100.25 | 37.2% | $0.14 |
| 18–24 | 246 | 7 | $1.65 | 0.6% | $0.24 |

### Meta Ads Geographic Breakdown (States)
| State | Impressions | Spend | % of Spend |
| :--- | :---: | :---: | :---: |
| Maine | 6,512 | $56.15 | 20.8% |
| Massachusetts | 6,239 | $54.64 | 20.3% |
| New York | 8,434 | $52.69 | 19.6% |
| Florida | 4,973 | $34.33 | 12.7% |
| Pennsylvania | 2,269 | $16.44 | 6.1% |
| New Hampshire | 1,870 | $15.47 | 5.7% |
| Connecticut | 1,608 | $13.49 | 5.0% |
| New Jersey | 1,044 | $7.72 | 2.9% |
| Maryland | 880 | $6.65 | 2.5% |
| Vermont | 824 | $6.05 | 2.2% |
| Rhode Island | 701 | $5.78 | 2.1% |

## 🌐 Google Analytics 4 (GA4) Traffic Overview
Top traffic channels landing on `lanterncamp.com` and `app.mews.com` sorted by total Sessions:

| Source / Medium | Active Users | Sessions | Registered GA4 Conversions | Revenue |
| :--- | :---: | :---: | :---: | :---: |
| Instagram / feed, static | 2,168 | 2,246 | 0 | $0.00 |
| (direct) / (none) | 698 | 780 | 0 | $0.00 |
| m.facebook.com / referral | 473 | 479 | 0 | $0.00 |
| google / organic | 214 | 408 | 0 | $0.00 |
| facebook.com / referral | 382 | 384 | 0 | $0.00 |
| instagram.com / referral | 196 | 198 | 0 | $0.00 |
| (not set) | 153 | 160 | 0 | $0.00 |
| instagram / carousel | 107 | 109 | 0 | $0.00 |
| l.instagram.com / referral | 77 | 78 | 0 | $0.00 |
| instagram / ad | 47 | 58 | 0 | $0.00 |
| ig / social | 45 | 55 | 0 | $0.00 |
| lanterncamp.com / referral | 30 | 49 | 0 | $0.00 |
| google / cpc | 36 | 43 | 0 | $0.00 |
| Instagram / influencer | 27 | 38 | 0 | $0.00 |
| tagassistant.google.com / referral | 2 | 16 | 0 | $0.00 |

## ⚠️ Tracking Health & Key Observations
> [!NOTE]
> ### 1. GA4 Purchase Event Status
> **Observation:** In the GA4 traffic data, there are currently **0 Conversions** and **$0 Revenue** recorded across all channels, including organic and paid.
> **Action Required:** GA4 is not currently tracking `purchase` events or transaction values. This typically occurs because a dedicated **GA4 Purchase Event tag** has not been configured in GTM (your GTM setup includes tags for Google Ads and Meta Pixel purchases, but lacks a specific GA4 Event tag for purchases). Setting up this tag will activate ecommerce reporting in GA4.

> [!IMPORTANT]
> ### 2. Link Clicks vs. Sessions Discrepancy
> * **Google Ads:** Reported **39 clicks**, but GA4 only recorded **43 sessions** with source `google / cpc` (~110.3% match).
> * **Meta Ads:** Reported **4,047 clicks** (paid campaigns), while GA4 recorded **2,414 sessions** from UTM matching sources (`feed, static`, `carousel`, etc.).
> * **Context:** Some drop-off is normal due to consent cookie declines, ad-blockers, and users bouncing before page load. However, a discrepancy this large (especially on Google Ads) indicates that the tracking config should be checked for page latency or tracking consent script blocks.

> [!WARNING]
> ### 3. UTM Naming Case-Sensitivity
> In GA4, UTM sources are case-sensitive. Traffic is split between `Instagram / feed, static` (Capital 'I') and `instagram / ad` or `instagram / carousel` (lowercase 'i'). Consolidating to lowercase UTM parameters in future Meta campaigns will prevent fragmented rows in GA4 reporting.

> [!WARNING]
> ### 4. Meta Ads Audience & Geo Target Skewing (Caveats)
> * **Age 65+ Skew:** The **65+ demographic represents 37.2% of Meta spend** ($100.25 of $269.40) and generated 716 LPVs. Although this is producing cheap views ($0.14/LPV), this demographic is highly unlikely to match the core guest profile for glamping/cabins.
> * **Local Maine Geo Focus:** **Maine spend represents 20.8% of total budget** ($56.15 of $269.41). If target customers are drive-market tourists from neighboring states (e.g. MA, NY), this heavy local spending in Maine represents targeting inefficiency.
> * **Targeting Action:** Switch off Advantage+ audience features to apply hard caps (such as age limits 25-54) and exclude/reduce local Maine targeting to shift budget to out-of-state drive markets.
