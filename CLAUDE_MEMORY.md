# Missed Rentals — Claude Session Memory

Use this file to restore context in a new Claude session. Paste the contents into the chat or say "read the memory file from the MissedRentals GitHub repo."

---

## What's built and working

- **Form:** Custom HTML form at `https://tawtechdabbling.github.io/MissedRentals/` — fully functional, no login required for staff
- **Form file (local):** `/Users/thomaswagler/Library/CloudStorage/OneDrive-DutchRentalz/Apps/Missed Rentals app/index.html`
- **GitHub repo:** `https://github.com/tawtechdabbling/MissedRentals`
- **Data storage:** Google Sheets — `https://docs.google.com/spreadsheets/d/1Um_qzSusR5z5Pmh9-C8eqe07_RDk3YrM/edit`
- **Google Sheet is set to:** Anyone with the link can view (required for Power BI)
- **Google Apps Script webhook:** `https://script.google.com/macros/s/AKfycbwQpW6ybNGXlYJxGuLtkJiJuVh6N5Za949zs-uEZ0pRu9aqD_aKJ1hqHtrmeSCx6c0Z4Q/exec`
- **Test submission confirmed working** — row appeared in Google Sheets after form submit

## Data columns in Google Sheets (Submissions tab)

Timestamp, StaffName, RentalDate, Item, Category, Duration, Quantity, Reason, Notes

## Power BI status

- Power BI Desktop installed on Windows
- Connected to Google Sheets via: `https://docs.google.com/spreadsheets/d/1Um_qzSusR5z5Pmh9-C8eqe07_RDk3YrM/export?format=xlsx`
- Submissions sheet loaded successfully
- **Paused at:** Checking RentalDate data type in Modeling tab

## Next steps

1. In Power BI → Modeling tab → click RentalDate → confirm data type is Date (not Text)
2. Decide how to handle missed revenue (price field on form, rate card lookup, or just count volume)
3. Build visuals: missed rentals by month/quarter/half-year, reasons breakdown, by team member
4. Publish to Power BI web (app.powerbi.com) so Tommy can view on browser

## Key decisions made

- Staff have no Microsoft accounts — form must be publicly accessible (no login)
- Rejected: Power Automate (premium URL required auth), SharePoint cookie auth, Make.com (permissions issues)
- Chose: Google Sheets as data store, GitHub Pages for form hosting
- No missed revenue dollar field yet — needs decision on calculation method

## GitHub Pages

- Enabled on `main` branch, root folder `/`
- Live URL: `https://tawtechdabbling.github.io/MissedRentals/`
