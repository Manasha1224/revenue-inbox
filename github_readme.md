# 📊 Revenue Intelligence Inbox

A clickable, production-grade workspace built for sales managers to triage high-volume customer interactions, detect revenue risks, and turn messy communication records into actionable daily priorities.

[![Live Demo](https://img.shields.io/badge/Demo-GitHub%20Pages-blue?style=for-the-badge&logo=github)](https://manasha1224.github.io/revenue-inbox/)
[![License](https://img.shields.io/badge/License-Private%20Assessment-lightgrey?style=for-the-badge)](LICENSE)
[![Date Scope](https://img.shields.io/badge/Date%20Scope-21%20Aug%202026-teal?style=for-the-badge)](https://manasha1224.github.io/revenue-inbox/)

---

## 🌐 Live Workspace

- **Hosted URL**: [https://manasha1224.github.io/revenue-inbox/](https://manasha1224.github.io/revenue-inbox/)
- **Date Scope**: 21 August 2026 (Asia/Kolkata)
- **Dataset Scope**: 44 Selected Records (30 Calls, 14 Meetings)

---

## 🚀 Quick Start & Local Setup

The application is completely self-contained with **zero external backend dependencies** or third-party paid API keys. All state management, import replays, and manual overrides persist locally via `localStorage`.

### Option 1: View Online
Open the live deployment directly in your browser:  
👉 **[Launch Application](https://manasha1224.github.io/revenue-inbox/)**

### Option 2: Run Locally
```bash
# Clone the repository
git clone https://github.com/manasha1224/revenue-inbox.git

# Navigate into the project folder
cd revenue-inbox

# Serve using any standard static server (e.g., Python 3)
python3 -m http.server 8000
```
Open `http://localhost:8000` in your browser.

---

## 🔄 Reproducing Data Contract & Replay Checks

The UI provides built-in replay controller buttons to test every verification stage:

| Step | Action in Interface | Verified Behaviour |
| :--- | :--- | :--- |
| **1. Initial Import** | Click **"Load Initial Batch (01_initial)"** | Ingests 44 distinct source records (30 calls, 14 meetings). Exactly 17 records display usable transcripts. Missing/silent audio records are visibly flagged as `Unprocessed / Low Signal`. |
| **2. Deduplication** | Click **"Re-import Initial (Deduplication Check)"** | Re-runs the initial batch (optionally with records shuffled). The record count strictly remains **44** (zero duplicate records, no count inflation). |
| **3. Manual Correction** | Select record **M07** (Willowfield Trust), change its route dropdown to **"Customer Support / Engineering Bug"**, and click **"Save Route Override"** | The manual correction is marked with an active `[Manual Override]` flag and persisted immediately. |
| **4. Late Update** | Click **"Apply Update Batch (02_update)"** | Ingests Revision 2 data for M07. Total unique records strictly stay at **44**. Usable transcript coverage increases from **17 to 18** (40.9%). M07's late transcript becomes readable. |
| **5. Preserve Judgment** | Refresh browser (F5) or restart app | M07's manual route **"Customer Support / Engineering Bug"** remains preserved and does not reset to defaults. New transcript evidence remains inspectable. |

---

## 📐 Metric Definitions & Formulas

Every metric follows safe calculation principles without conflating unverified metadata:

- **Total Selected Records (44)**: Distinct count of logical `(source_type, source_id)` pairs across ingested deliveries.
- **Usable Transcript Coverage**:
  $$\text{Coverage} = \frac{\text{Records with usable non-empty text}}{\text{Total selected records}}$$
  - Initial Batch: $17 / 44$ (38.6%)
  - Post-Update Batch: $18 / 44$ (40.9%)
- **Source-Reported Connection Rate (36.7%)**:
  $$\text{Connection Rate} = \frac{\text{Calls with } \text{is\_connected} = \text{true}}{\text{Calls with non-null } \text{is\_connected}} = \frac{11}{30}$$
  - *Governance Note*: Labeled strictly as "Source-Reported", not verified ground truth (e.g., phantom calls B11 and B14 report `is_connected=true` despite having zero text).
- **Needs Review / Triage Count**: Records pending managerial classification review, pending overrides, or containing unresolved customer blockers.

---

## 🔍 Key Findings & Deliberate Decisions

### 3 Most Important Analytical Findings
1. **Critical Defect Escalation (M07 — Willowfield Trust, Turns 3–25)**:
   - Calendar title labeled as a routine meeting, but the conversation revealed 4 severe platform bugs (description edit not saving, file upload crashing on recreate flow, payouts frozen in "Processing" for >48 hours, and stale payment amount retention).
   - *Product Impact*: Correctly routed to **Customer Support / Engineering Bug**, bypassing routine sales review.
2. **Product Expectation Mismatch (M03 & M04 — Asterloom & Cedarwave)**:
   - Asterloom (Turn 6: *"I had thought about a corporate credit card"*) and Cedarwave (Turn 11: *"Moving from prepaid to credit is the main issue for us"*) both stalled due to credit facility demands. SpendNest's prepaid model created friction.
   - *Product Impact*: Flagged with a high-priority "Product Positioning Gap" tag in the manager daily brief.
3. **Strategic Supplier vs Prospect Distinction (M02 — LedgerBridge, Turns 1–31)**:
   - Upstream logged as an external call, but transcript establishes LedgerBridge is a compliant e-invoicing vendor pitching middleware to SpendNest.
   - *Product Impact*: Routed to **Internal & Vendor Strategy** to prevent pipeline inflation.

### Deliberately Rejected Decision
- **Rejected Date Range Filtering**:
  - *Alternative Considered*: Implementing a traditional calendar/date filter on the dashboard.
  - *Reason for Rejection*: All 44 provided records fall strictly on a single local date (**21 August 2026, Asia/Kolkata**). A date filter would introduce empty states and user confusion.
  - *Trade-off Accepted*: Prioritized multi-factor person (Rep / Organizer) and source-type (Call vs Meeting) filters instead.

---

## 📂 Export Structure & Machine-Readable Manifest

Clicking **"Export Manifest & Records"** generates an export package containing:
- `manifest.json`: Machine-readable audit file containing:
  - `source_type`
  - `source_id`
  - `applied_revision`
  - `route`
  - `output_path` (relative file paths inside `/outputs/deals/`, `/outputs/support/`, `/outputs/coaching/`, `/outputs/vendor/`)
  - `review_state`
  - `manual_override` (`true`/`false`)
  - `analysis_freshness` (`current`/`stale`)

---

## ⚙️ Architecture, AI Usage & Effort

- **Analysis Architecture**: All 18 transcript syntheses and the August 21 Manager Daily Brief are structured and pre-generated to eliminate runtime LLM latency, paid API dependencies, and risk of hallucination during reviewer testing.
- **Deterministic Engine**: Deduplication, revision resolution (higher revision wins), and manual route preservation are enforced by strict deterministic JavaScript logic adhering to `DATA_CONTRACT.md`.
- **Approximate Time Spent**: ~7 hours total across requirement analysis, data contract engine implementation, UI development, testing, slide deck design, and documentation.