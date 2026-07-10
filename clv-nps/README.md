# CLV-NPS — The VIP Paradox

## The problem

A repeat-purchase business tracks Customer Lifetime Value to spot who's worth protecting, and pairs it with NPS to catch dissatisfaction before it costs a sale. Most tools score these two things separately and never check whether they agree. If a customer's revenue math looks fine, nobody looks any further — even when there's a live warning sign sitting one sheet over.

## The mechanism

The engine turns six inputs per customer (segment, average order value, purchase frequency, lifespan, gross margin %, acquisition cost) into Basic CLV, Margin-Adjusted CLV, Payback Period, and a CLV:CAC Ratio — then raises a Churn Risk flag whenever CLV:CAC drops below 1.5x *or* Payback runs past 24 months. A separate NPS sheet tracks sentiment by segment (Promoter / Passive / Detractor). Built in Google Sheets + Excel VBA — no CRM, no data team.

## Sample data

[`sample-data/clv-customer-data.csv`](sample-data/clv-customer-data.csv) — 50 customers, illustrative synthetic data for a subscription/repeat-purchase business, not a real client. [`sample-data/clv-nps-responses.csv`](sample-data/clv-nps-responses.csv) — 68 NPS responses across the same three segments. Columns match the bundle's actual input schema exactly — every computed column (Basic CLV, Margin-Adjusted CLV, Payback, CLV:CAC, Churn Risk) is calculated by the engine, not entered by hand.

## What the output shows

Run through the engine, this dataset surfaces a real blind spot worth knowing about before you trust the Churn Risk flag: **10 customers (20% of the base), holding $25,614 — 51.6% of total Margin-Adjusted CLV — can never be flagged Churn Risk, no matter how they're actually doing.**

The reason is mechanical, not a bug in the data: these customers were acquired for $0 (referral, word-of-mouth, organic). Both CLV:CAC Ratio and Payback Period are defined as *something* ÷ CAC — when CAC is 0, the sheet's own guard logic leaves both cells blank rather than dividing by zero. The Churn Risk formula then checks `(CLV:CAC < 1.5) + (Payback > 24) > 0` — but a blank cell fails `ISNUMBER()`, so both terms silently evaluate to zero. The flag reads **STABLE**, not because the tool checked and found them healthy, but because it was never able to check at all. Meanwhile, 8 paid-acquisition customers with a genuinely thin CLV:CAC ratio *do* trip the flag correctly — the tool isn't broken, it's just structurally blind to anyone who cost nothing to acquire.

The NPS sheet tells the other half of the story. These $0-CAC customers all sit in the "High Value" segment — and High Value is the *worst*-scoring segment in the whole dataset: NPS of **-30.0** (50% Detractors), against -8.3 for Mid Value and 0.0 for Low Value. Real dissatisfaction exists in exactly the segment carrying the CAC-blind cohort — the Churn Risk flag simply has no wiring to it, because Churn Risk never reads the NPS sheet at all.

**Practical read:** "STABLE" on this tool means "computable and above threshold" — not "healthy." Any customer with CAC = 0 needs a manual NPS/engagement check on a schedule, because the engine's own risk math will never flag them, positively or negatively. This is the mirror image of a normal blind spot: it's not that the tool misjudges these customers, it's that its highest-value segment is systematically invisible to its own early-warning system.

*Illustrative synthetic data — not a real client result.*

## Read the plain-English version

[contentsimplify.com/case-studies/clv-nps-model](https://contentsimplify.com/case-studies/clv-nps-model)
