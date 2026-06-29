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

## Item Rate Sheet

- Tab name in Google Sheets: **Item Rate Sheet**
- Columns: Description, Item Category, Four Hours, Day, Week, 4 Week
- Item names in the rate sheet match exactly what staff pick on the form
- Form Duration values: `4 Hours`, `Day`, `Week`, `4 Weeks` (note: form says "4 Weeks", rate sheet column says "4 Week")

## Power BI status

- Power BI Desktop installed on Windows
- .pbix file exists and was found/opened successfully
- Connected to Google Sheets via: `https://docs.google.com/spreadsheets/d/1Um_qzSusR5z5Pmh9-C8eqe07_RDk3YrM/export?format=xlsx`
- **Submissions** table loaded — no real data yet (only test submissions), but table is present
- **Item Rate Sheet** table loaded as second table via same export URL
- **Relationship created:** Submissions[Item] → Item Rate Sheet[Description], active, cross filter both directions

## Next steps

1. **Verify exact table/column names in Power BI** — got an error adding DAX column because Power BI may have named the table/columns slightly differently than expected. Need to check exact names in the Fields panel before writing the DAX.
2. **Add DAX calculated column** for Missed Revenue — in Data view, select Submissions table, New Column, paste this DAX (substituting correct table/column names):
   ```
   Missed Revenue = 
   VAR rate =
       SWITCH(Submissions[Duration],
           "4 Hours", RELATED('Item Rate Sheet'[Four Hours]),
           "Day",     RELATED('Item Rate Sheet'[Day]),
           "Week",    RELATED('Item Rate Sheet'[Week]),
           "4 Weeks", RELATED('Item Rate Sheet'[4 Week])
       )
   RETURN rate * Submissions[Quantity]
   ```
   Note: The table name in DAX may need quotes if it has spaces, e.g. `'Item Rate Sheet'`. Also need to confirm the Submissions table column is called `Duration` (not something else).
3. Build visuals: missed rentals by month/quarter/half-year, reasons breakdown, by team member, total missed revenue
4. Publish to Power BI web (app.powerbi.com) so Tommy can view on browser

## Key decisions made

- Staff have no Microsoft accounts — form must be publicly accessible (no login)
- Rejected: Power Automate (premium URL required auth), SharePoint cookie auth, Make.com (permissions issues)
- Chose: Google Sheets as data store, GitHub Pages for form hosting
- No price field on the form — missed revenue is calculated by looking up the item rate from the Item Rate Sheet using the submitted Duration and Quantity
- Rate lookup will be done in Power BI via DAX (not Google Sheets formulas) to keep raw data clean

## GitHub Pages

- Enabled on `main` branch, root folder `/`
- Live URL: `https://tawtechdabbling.github.io/MissedRentals/`
