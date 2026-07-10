# Output — pending

This folder holds the RFM Segment Summary Dashboard screenshot and the calculated RFM Scoring export (with AOV / Days Since / R / F / M / Composite / Segment / Churn Risk columns filled in).

To generate:
1. Paste the debug-complete Pro-tier GAS code (`RFM MODEL Bundle Script Pro ($29) Final.md`) into a fresh Google Sheet's Apps Script editor and run the build function.
2. Paste `../sample-data/rfm-customer-data.csv` into the generated "Raw Transaction Data" tab (columns A-E only).
3. Re-run `Rebuild RFM Scoring` from the menu.
4. Screenshot the "Segment Summary Dashboard" tab → save here as `dashboard-screenshot.png`.
5. Export "RFM Scoring" as CSV → save here as `rfm-scoring-calculated.csv`.
