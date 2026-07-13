# Output — pending

This folder holds the Growth Accounting Dashboard screenshot and the calculated Cohort Data export (with Activation/Retention/Referral/Monetization/Churn rate + flag columns and Health Score filled in).

To generate:
1. Paste the debug-complete Pro-tier GAS code (`AARRR-RARRA MODEL Bundle GoCode Script $29 Final.md`) into a fresh Google Sheet's Apps Script editor and run the build function.
2. Set Control Center `ControlRetentionWindow` to D30 and `ControlGrowthFocus` to Retention.
3. Paste `../sample-data/aarrr-cohort-data.csv` into the generated "Raw Cohort Data" tab.
4. Screenshot the "Growth Accounting Dashboard" tab → save here as `dashboard-screenshot.png`.
5. Export "AARRR Calculations" as CSV → save here as `aarrr-calculated.csv`.
