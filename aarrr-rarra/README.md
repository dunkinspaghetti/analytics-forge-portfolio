# AARRR-RARRA — The Funnel Blind Spot

## The problem

A small SaaS/app team tracks Acquisition, Activation, Retention, Referral, and Revenue, but usually as five separate spreadsheets nobody looks at together — or, when they do use a single "Health Score," they trust one composite number to say whether growth is healthy. That number is only useful if every stage feeding it means what the team thinks it means.

## The mechanism

AARRR-RARRA scores every cohort (one row per period × acquisition channel) across all five stages, applies traffic-light thresholds per stage, and rolls them into a weighted 0–100 Health Score (Activation 25pts + Retention 30pts + Referral 15pts + Monetization 20pts + Churn 10pts). A Weakest Stage column flags which lever to pull next. Built in Google Sheets + Excel VBA — no dashboard software, no data team.

## Sample data

[`sample-data/aarrr-cohort-data.csv`](sample-data/aarrr-cohort-data.csv) — 8 monthly cohorts (Jan–Aug 2026) × 3 acquisition channels, illustrative synthetic data, not a real client.

| Column | What it captures |
|---|---|
| Cohort Period | Reporting month |
| Acquisition Channel | Organic Search / Paid Search / Community |
| New Visitors / Installs | New users this cohort |
| Activated Users | Completed the key activation event |
| Retained Users (D30) | Still active at the 30-day window |
| Referrals Sent | Invitations/shares generated — blank means no referral program run that period, not zero |
| Referral Conversions | New users acquired via referral |
| Paying Users | Unique users who paid ≥1 time |
| Revenue | Net revenue for the period |
| Churned Users | Cancellations + inactives beyond the retention window |

## What the output shows

Two channels tie at an identical **Health Score of 85/100**, every single month — but they are not in the same situation:

- **Organic Search** has never run a referral program. Referrals Sent/Conversions are blank — not zero, unmeasured. Its Referral Flag reads `—`.
- **Paid Search** *is* running a referral program, and it's failing: a ~3% referral rate, well under the tool's own 5% critical threshold. Its Referral Flag reads `🔴`.

The schema documentation states plainly that stages with no data "contribute 0 pts (not penalised for missing optional fields like Referral)" — but the Health Score weighting table right next to that sentence assigns 0 points to *both* a blank referral field and a `🔴` (<5%) referral rate. There is no third bucket. A channel that hasn't tried referral yet and a channel whose referral program is actively broken are mathematically indistinguishable in the one number the dashboard hands the owner to act on. **Community**, by contrast, scores 57 (Monitor band) — genuinely weaker across Activation and Retention, correctly reflected.

Practically: an owner glancing at "Organic Search: 85, Paid Search: 85" would treat them the same. One needs a referral program built from scratch (a growth opportunity); the other needs its existing referral program diagnosed and fixed (an active problem). The Health Score can't tell them apart — only reading the Referral Flag column directly (`—` vs `🔴`) does. See [`output/`](output/) for the dashboard screenshot and calculated export once run.

*Illustrative synthetic data — not a real client result.*

## Read the plain-English version

[contentsimplify.com/case-studies/aarrr-rarra-model](https://contentsimplify.com/case-studies/aarrr-rarra-model)
