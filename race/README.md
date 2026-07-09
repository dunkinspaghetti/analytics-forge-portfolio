# RACE — Click-to-Cash Leak Detector

**The problem:** A business runs campaigns across multiple channels every month. Budget flows to whatever feels active. One channel quietly burns spend without converting — and there's no structured way to compare channels side by side without a data team.

**The engine:** RACE maps every channel to four funnel stages — Reach, Act, Convert, Engage — and calculates five KPIs automatically from one input row per channel per period. A built-in bottleneck detector flags the worst-performing stage per channel and surfaces the exact AI diagnostic prompt to fix it.

---

## Input schema

One row per channel per time period. The engine handles any number of channels and any time range.

| Column | Type | Description |
|---|---|---|
| Period | Text | e.g. `Jan 2025` — consistent format required |
| Channel | Text | e.g. `Paid Social`, `Google Ads`, `Email Marketing` |
| Ad Spend | Currency | 0 for organic channels |
| Reach / Impressions | Integer | Total impressions, visits, or list size |
| % New Audience | 0–100 | Percentage of new vs returning audience |
| Interactions / Clicks | Integer | Clicks, link taps, or direct engagements |
| Micro-Conversions (Leads) | Integer | Form fills, sign-ups, trial starts |
| Macro-Conversions (Sales) | Integer | Actual purchases or paid conversions |
| Revenue Generated | Currency | Total revenue attributed to the channel |
| Repeat Buyers | Integer | Customers who purchased more than once in period |

Sample data: [`sample-data/race-campaign-data.csv`](./sample-data/race-campaign-data.csv) — 108 rows, 9 channels, 12 months (Jan–Dec 2025).

---

## Computed output

The engine derives five KPIs per row and assigns a health flag against calibrated thresholds:

| KPI | Formula | 🟢 | 🟡 | 🔴 |
|---|---|---|---|---|
| Act Rate | Interactions ÷ Reach | ≥ 3% | 1–3% | < 1% |
| Micro-CVR | Leads ÷ Interactions | ≥ 10% | 3–10% | < 3% |
| Macro-CVR | Sales ÷ Leads | ≥ 2% | 0.5–2% | < 0.5% |
| ROAS | Revenue ÷ Ad Spend | ≥ 4× | 2–4× | < 2× |
| Loyalty Rate | Repeat Buyers ÷ Sales | ≥ 20% | 10–20% | < 10% |

A composite **health score** (0–100) weights each KPI at 20 points. The engine flags the weakest RACE stage per channel and surfaces the corresponding AI prompt.

---

## Sample findings (Jan 2025, illustrative data)

| Channel | Act Rate | Micro-CVR | Macro-CVR | ROAS | Loyalty | Health |
|---|---|---|---|---|---|---|
| Paid Social | 4.0% 🟢 | 18.9% 🟢 | 26.1% 🟢 | 1.45× 🔴 | 13.0% 🟡 | 70 |
| Google Ads | 6.0% 🟢 | 28.9% 🟢 | 35.4% 🟢 | 4.88× 🟢 | 21.7% 🟢 | 100 |
| Email Marketing | 8.0% 🟢 | 29.9% 🟢 | 30.4% 🟢 | 10.7× 🟢 | 40.0% 🟢 | 100 |
| SEO & Content | 5.0% 🟢 | 30.2% 🟢 | 25.3% 🟢 | 3.28× 🟡 | 23.8% 🟢 | 90 |
| Direct / Brand | 6.0% 🟢 | 29.8% 🟢 | 30.0% 🟢 | 3.21× 🟡 | 40.0% 🟢 | 90 |
| YouTube Ads | 1.1% 🟡 | 14.9% 🟢 | 25.0% 🟢 | 1.20× 🔴 | 22.2% 🟢 | 70 |
| Affiliate / Referral | 8.0% 🟢 | 30.1% 🟢 | 29.9% 🟢 | 11.5× 🟢 | 17.4% 🟡 | 90 |
| LinkedIn Ads | 3.0% 🟢 | 25.4% 🟢 | 31.3% 🟢 | 2.57× 🟡 | 20.0% 🟢 | 90 |
| WhatsApp / SMS | 60.0% 🟢 | 35.0% 🟢 | 29.8% 🟢 | 17.6× 🟢 | 40.0% 🟢 | 100 |

**The finding:** Two channels flagged 🔴 ROAS — Paid Social (1.45×) and YouTube Ads (1.20×). Both clear their Act Rate and Micro-CVR thresholds, meaning audiences click and convert to leads — but neither channel turns spend into revenue at a defensible rate. Combined January spend: $1,250. Combined January revenue: $1,700. Blended ROAS: 1.36×, against a portfolio average of 3.85×.

Paid Social's weakest RACE stage: **Convert** (ROAS 🔴). YouTube's weakest stage: also **Convert** — with the added signal that its Act Rate (1.1%) sits in 🟡 territory, meaning the audience isn't clicking cleanly before the revenue shortfall compounds.

A side finding that emerged across the full 12-month run: SEO & Content started January at 3.28× ROAS (🟡) and compounded to 4.88× (🟢) by December — the only channel in the dataset where health improved meaningfully without a budget increase.

> Illustrative synthetic data — not a real client result.

---

## What's in this folder

```
race/
  README.md                     — this file
  sample-data/
    race-campaign-data.csv      — 108 rows, 9 channels, Jan–Dec 2025
```

The full 6-sheet Google Sheets / Excel VBA engine is available as a productized service.
→ [Case study + service details](https://contentsimplify.com/case-studies/race-model)

---

## Tech stack

- **Google Sheets** — 6-tab engine: Control Center → Raw Data → Data Cleaning → Calculations → SOSTAC Planner → Dashboard
- **Google Apps Script** — single-file builder (`RACE_Analytics_Engine_Complete.gs`), SOLID + DRY architecture
- **Excel VBA** — equivalent engine for Windows / Office environments
- **Design notes** — ArrayFormula KPI computation, named-range ownership contract (each sheet owns its ranges; Dashboard never touches Calculations ranges), division-guarded denominators, AVERAGEIFS over computed columns (QUERY is unreliable on ArrayFormula output)
