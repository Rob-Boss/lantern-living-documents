# Lantern Camp Workspace Tracker

Use this document to track assignments, to-dos, hygiene, and projects. 

---

## ⚡ Short-Term (Immediate / This Week)

### 📋 To-Dos & Assignments
*   [x] **Traveling Mainers Contract:** Evaluated and decided it was not a good deal (not moving forward).
*   [x] **Send Resume:** Email Addison your resume (completed).
*   [x] **Estimate Hours:** Provided estimate of 15 hours per week.
*   [x] **Share Standard Contract:** Shared standard contract with Addison (completed).
*   [x] **Confirm Compensation:** Confirmed compensation ($3,500 monthly retainer).
*   [x] **Contract Signature:** Addison and Tony signed; contract is finalized.
*   [x] **Invoice Payment:** June retainer invoice paid (received today).
*   [x] **PR Outreach & Evaluation:** Track and evaluate agencies in [pr-agency-evaluation.md](#/docs/pr-agency-evaluation) (completed initial calls and received all final pricing/capacity updates).
*   [x] **Upspring Team Call:** Follow-up call completed. Tony is ready to move forward; Addison is investigating further with reservations around how closely aligned the PR agency needs to be with our rustic glamping concept.
*   [x] **Michelle Lawrence Visit & Shot List:** Refined shot list and completed her visit (real-time stories posted successfully).
*   [ ] **Michelle Lawrence Wednesday Post Campaign:**
    *   [x] **Content Preview:** Reviewed her proposed carousel media and copy. Copy is really good, and media is amazing. Identified must-have and favorite media for Tony and Addison.
    *   [x] **Go Live (Wednesday/Thursday):** Media selections and copy tweaks (swapping list items #2 & #4, adding dog-friendly info) approved by Addison and Tony. Agreed on a Collab Post. Sent UTM tracking link (https://bit.ly/3SAb5eZ) to Michelle for her bio link. Post is expected to go live today.
    *   [ ] **Instagram Native Boost (Ad Code Received):** Boost the Collab Post directly using Michelle's partnership ad code (`adcode-Q9jTBBX3MltygKh25ZgBuxJxXEeyUtItDbqs77jKHndCBSE_3X9hmtWVpGaUrueZGBU`) from Meta Ads Manager once organic traffic peters out (1-2 days).
*   [x] **First Newsletter Outline:** Sent first concept/outline (*"3 Days in Acadia: A Low(er) Traffic Itinerary"*) to Addison & Tony (approved with notes).
*   [ ] **Write First Newsletter:** Write first newsletter based on the approved Acadia concept (incorporating notes).
*   [ ] **Brainstorm Newsletter Topics:** Brainstorm 2-3 additional topics to establish a bi-weekly content pipeline.
*   [/] **Create Meta Ad (Offline due to Suspension):** Published first standalone static ad (using "Field Cabin 1" photo and safe copy) to test baseline performance. (Ad campaign is currently offline again due to bank payment blockages; waiting on Addison to switch payment to PayPal or contact bank). Plan to add alternative photos and copy variations over time.
    *   [x] **Clip Library:** Sourced wide-format video clips from Peter Logue (beautiful clips, but wide framing makes ad reformatting a challenge; great for adding website motion).
    *   [ ] **Video Edit / Recap:** Wait for John (Peter's partner) to create a ~1-minute recap/ad video (delayed a few weeks due to travel).
    *   [x] **Draft Ads / Static Ad Launch:** Published first standalone ad using "Field Cabin 1" and safe copy to kick off testing. Keep testing/adding alternate copy/creative.
*   [/] **Centralized Analytics Dashboard:** Build an analytics dashboard integrating GA4, UTM links, GTM, Meta Pixel, Meta Ads campaigns, Mews, Airbnb, and Booking.com to track bookings, ad attribution, and compare spend vs. revenue. (Cloud setup in progress; do not pull local code changes for now).

### 🧼 Hygiene & Setup
*   [x] **Dropbox Folder Access:** Accessed `"Photos + Videos_Working"` Dropbox folder.
*   [x] **Google Sheet Access:** Accessed `"LC Community Relationships.xlsx"` Google Sheet.
*   [x] **Instagram Credentials:** Retrieved and saved Instagram credentials from 1Password link.
*   [x] **Squarespace Access:** Retrieved access to the Squarespace website (eventually for Meta Pixel setup).
*   [x] **Google Analytics Embed:** GA4 account access verified and tag `G-3JJ6S8J5ZD` integrated via Google Tag Manager.
*   [x] **Folder/File Ownership:** Completed file/folder transition from Tiffany (outreach list and other documents received).
*   [x] **Meta Business & Ads Setup:**
    *   [x] **Create Business Portfolio:** Created business manager/portfolio under Addison's personal Facebook account.
    *   [x] **Create Lantern Camp Page:** Page setup successfully created with Meta Support on June 11.
    *   [x] **Add Partner/Collaborator:** Addison invited Ben to join the "Addison's Businesses" portfolio as a partner (invite received June 11).
    *   [x] **Meta Verification:** Addison successfully completed Meta Verification.
    *   [x] **Resolve Setup Snags:** Addison successfully worked with Meta Support on June 11 to troubleshoot portfolio and page creation.
    *   [x] **Verify Portfolio Access (High Priority):** Accept the partner invite received via email on June 11 and verify access.
    *   [x] **Create Lantern Camp Ad Account (High Priority):** Spin up an ad account associated with Lantern Camp (ID: 1585226713251054).
    *   [x] **Install Meta Pixel:** Installed Base Meta Pixel (ID: 2012732439334242) and integrated custom Purchase events via GTM on Mews Booking Engine.
    *   [x] **Select AI Ad Platform (MCP vs. Ryze):** Selected Meta's native MCP (Advantage+) to run the ad account. (Confirmed working fine; not using Ryze).

---

## 🎛️ Meta Ads & GTM Tracking Integration

All technical architecture, infrastructure settings, tags, triggers, and unresolved flags are documented in [tracking-and-analytics-setup.md](#/docs/tracking-and-analytics-setup).

### 📋 Verification & Cleanup Roadmap
*   [ ] **Mews Backend Housekeeping & Cleanup (High Priority):**
    *   [x] **Locate and cancel test reservations:** Cancelled all 5 test reservations (#17, #18, #19, #22, plus the fifth) in the Mews operations portal to restore cabin inventory. (Used refundable option; fully refunded).
    *   [ ] Fix system configuration typo: Change `item_category3` layout attribute from "Orlando, FL" to "Orland, Maine".
    *   [ ] (Optional) Remove duplicate unused pixels in the Meta account.
*   [/] **Mews Booking Notifications:** Resolve issue where booking notifications are silent. Addison connected Ben with the MEWS account manager (William) on June 16; Ben sent an initial email troubleshooting the access constraints and is waiting for a response.
*   [x] **Fix Meta Purchase Value & Currency:** Resolved GTM mapping issue. Replaced the Meta Pixel Purchase tag with a Custom HTML tag that scrapes the Mews dataLayer directly at runtime. Verified USD currency and purchase values are passing correctly via Facebook Pixel Helper.
*   [ ] **Live Guest Checkout Audit & Tracking Verification:**
    *   [ ] Monitor the Ads Manager Results column on the first real booking post-launch. Confirm it increments by 1 Purchase (verifying value-less Purchase conversion counting).
    *   [x] Verify first organic booking parameters inside Meta Events Manager (confirmed value and currency tracking resolved and verified via Facebook Pixel Helper).
    *   [ ] Customize Ads Manager reporting columns to display Purchases, Conversion Value, and ROAS side-by-side.

---

## 📅 Medium-Term (Next Few Weeks)

### 📋 To-Dos & Assignments
*   [ ] **Hire Assistant (Lucy Larson):** Lucy is interested in coming on for 10 hours/week starting June 18th to help with second-tier priorities.
    *   [ ] Finalize hiring and onboarding details with Lucy.
    *   [ ] Plan delegation of tasks (like outreach list processing).

### 🚀 Projects & Strategy
*   [/] **PR Strategy & Decision (Evaluating new paths):** After initially moving to proceed with Upspring, we decided **not** to move forward with them following an internal review on June 19. Current focus is on evaluating Carla Tracy and the option of doing PR in-house. Current state:
    *   [x] **Internal Alignment Meeting:** Completed June 19. Decided to pause/cancel Upspring.
    *   [x] **Call with Nancy Marshall (Marshall PR):** Completed June 19. Nancy advised that Upspring is a bad idea and recommended finding someone who understands Maine and Maine hospitality. She is connecting us with Carla Tracy.
    *   [ ] **Evaluate Carla Tracy:** Awaiting introduction from Nancy Marshall. (Maine-based solo publicist; Nancy guessed she might be $5k/mo, but suggested doubling to $10k for the first month to guarantee full attention; we haven't talked to her yet).
    *   [ ] **Evaluate In-House PR:** Assess feasibility of Swardy handling the PR sprint directly with a ~$12k budget and delegating secondary tasks to Lucy.
    *   [ ] **Founder Pitch Package (Debra Spark):** Draft founder-perspective pitch materials (story angle, copy, photos) and review with Debra Spark for feedback and warm intros to editors at *Decor Maine*, *Downeast*, and *Maine Home + Design*.
    *   [ ] **April (Maine Famous):** Await response to outreach to April to discuss potential promotion collaboration.
    *   [x] **Evaluate Caroline/Upspring POC:** Confirmed that Caroline will support strategy, while Delaney will be the day-to-day lead.
    *   [x] **Hospitality PR Consultation (Allie Gill):** Spoke with Allie Gill (Marketing at Uncommon Hospitality / Longfellow Hotel). She confirmed that sector overlap is not a concern (PR is a megaphone/relationship engine, not the brand voice) and strongly endorsed PR as the top priority for a hospitality launch (citing their success with writers/visitor stays). Recommended a light negotiation on the fee.
    *   [x] **Upspring Negotiation:** Sent counter-proposal of $5k/month for 4 months on June 11. UpSpring declined the rate reduction (noting $6.5k is already heavily discounted from their typical $10k starting rate) but offered to include their AI Audit (AI Visibility Score, prompt breakdown, citation map) for the $6.5k/mo fee.
    *   [x] **Kubany PR Intro Call:** Completed call on June 12. Extremely strong on traditional prestigious comms and design/architecture storytelling (great chemistry with Addison); weak on influencer marketing and AI/AI search visibility.
    *   [x] **Recommend Liz Kubany:** Aligned with Addison on recommendation (preferring boutique design focus and strategic leadership over UpSpring's quantity-over-quality model). Sent recommendation email to Tony on June 15 comparing competitive costs ($24K total for 3 months vs $26K total for 4 months with UpSpring).
    *   [x] **Resolve Partner Split (Tony vs Ben/Addison):** Resolved. Liz Kubany withdrew due to a family medical emergency. Met with Upspring (Delaney and Catherine) on June 16 to address communication concerns, and decided to move forward with Upspring.
*   [ ] **Newsletter Content Pipeline:** Establish a structured content calendar aiming for a bi-weekly cadence.
*   [ ] **Google AdWords / Sonia Engagement:** Discuss Sonia's proposal to own a project (AdWords/Email Campaigns) at $50/hr. Set hours/budget limits and define the reporting structure if approved.
*   [ ] **Outreach & Directory Listings (Waiting on Assistant):** Work through the transitioned list of organizations, affiliations, listings, and local businesses to get Lantern Camp listed (deferred until Lucy Larson starts).

---

## 🗺️ Long-Term (Months / Future)

### 🚀 Projects & Strategy
*   [ ] **Launch Marketing Campaign:** Plan and execute the launch marketing campaign with the $50-75k budget.
*   [ ] **Community Relationship Tracking:** Maintain and expand the LC Community Relationships database.
