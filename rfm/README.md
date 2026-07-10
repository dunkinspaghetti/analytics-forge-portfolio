# RFM — Which Customers Deserve Your Effort

## The problem

A repeat-purchase business (retail, online store, wholesale) builds up a customer list of dozens or hundreds of names, but has no ranked view of who is actually worth the next marketing dollar. Everyone gets the same email blast, the same discount code, the same follow-up call — while a handful of high-value customers quietly go silent and a pile of one-time buyers get treated the same as anyone else.

## The mechanism

RFM scores every customer on three dimensions — **R**ecency (days since last purchase), **F**requency (total orders), **M**onetary (total revenue) — each on a 1-5 scale, from one row of input per customer (last purchase date, order count, lifetime revenue). The three scores sum into a composite (3-15) that maps to one of 7 segments (Champion → Lost), and a separate Churn Risk flag fires when Recency is weak *and* the composite is low. Built in Google Sheets + Excel VBA — no CRM, no data team.

## Sample data

[`sample-data/rfm-customer-data.csv`](sample-data/rfm-customer-data.csv) — 52 customers, illustrative synthetic data for an online storefront, not a real client. Columns match the bundle's actual input schema (Customer ID, Customer Name, Last Purchase Date, Total Purchases, Total Revenue) — every other column (AOV, R/F/M scores, composite, segment, churn flag) is calculated by the engine, not entered by hand.

## What the output shows

Run through the engine, this dataset surfaces a real limitation worth knowing about before you trust the tool's segment labels blindly: **6 customers, holding $43,129 (33.6% of total revenue), are labeled "Loyal Customer" and never trip the Churn Risk flag — despite not having ordered in 9 to 15 months.**

The reason is mechanical, not a bug in the data: these customers hit rock-bottom Recency (score 1, >180 days silent) but still carry the maximum Frequency and Monetary scores (5 and 5) from a strong purchase history. The composite (1+5+5=11) lands comfortably in the "Loyal Customer" band (10-12) and clears the Churn Risk threshold (which only fires when composite ≤ 5) — so a customer who spent $8,900 across 13 orders and then vanished for 317 days reads exactly the same as someone who ordered last month.

The dashboard's own KPI panel tells the same story from a different angle: Avg RFM Composite lands at 8.27 (🟢 Healthy by the ≥8.0 benchmark), while Avg Recency Score sits at 2.75 and Avg Frequency Score at 2.77 — both 🔴 Critical (<3.0 threshold). The headline number says the customer base is healthy; the two components that actually measure *engagement* say otherwise. Monetary history is propping up the average.

**Practical read:** don't act on the composite score or the segment label alone for any customer who was ever a heavy spender. Sort by Recency score independently, especially inside "Loyal Customer" and "Champion" — that is where a real at-risk cohort hides in plain sight. This dataset also has a second, smaller cohort correctly caught by the flag (11 customers, ~7% of revenue, low across all three dimensions) — the tool works as designed there; it's the *former* high-value customer, not the mediocre one, that the composite formula misses.

*Illustrative synthetic data — not a real client result.*

## Read the plain-English version

[contentsimplify.com/case-studies/rfm-model](https://contentsimplify.com/case-studies/rfm-model)
