# Output — pending

This folder holds the CLV-NPS Dashboard screenshot and the calculated Data Cleaning export (with Basic CLV / Margin-Adj CLV / Payback / CLV:CAC / Churn Risk / NPS Category columns filled in).

To generate:
1. Paste the debug-complete Pro-tier GAS code (`CLV-NPS MODEL Bundle Script Pro.md`) into a fresh Google Sheet's Apps Script editor and run `buildEngine()`.
2. Paste `../sample-data/clv-customer-data.csv` into the generated "Raw Customer Data" tab (columns A-F only) and `../sample-data/clv-nps-responses.csv` into "NPS Responses" (columns A, B, C, G only).
3. Re-run `buildCalculationsSheet()` from the menu.
4. Screenshot the "Dashboard" tab → save here as `dashboard-screenshot.png`.
5. Export "Data Cleaning" as CSV → save here as `clv-nps-cleaning-calculated.csv`.
