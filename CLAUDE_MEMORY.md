# Missed Rentals — Claude Session Memory

Use this file to restore context in a new Claude session. Paste the contents into the chat or say "read the memory file from the MissedRentals GitHub repo."

---

## What's built and working

- **Form:** Custom HTML form at `https://tawtechdabbling.github.io/MissedRentals/` — fully functional, no login required for staff
- **Form file (local):** `C:\Users\Tommy\OneDrive - Dutch Rentalz\Apps\Missed Rentals app\index.html`
- **GitHub repo:** `https://github.com/tawtechdabbling/MissedRentals`
- **CORRECT Google Sheet ID:** `1rM1Zn_lvD9TzDImPW8Q3JmyWVhfrMEPH_20vTJOikO8`
- **Google Sheet URL:** `https://docs.google.com/spreadsheets/d/1rM1Zn_lvD9TzDImPW8Q3JmyWVhfrMEPH_20vTJOikO8/edit`
- **Google Sheet is set to:** Anyone with the link can view (required for Power BI)
- **Google Apps Script webhook (current):** `https://script.google.com/macros/s/AKfycbzi1PfswCr8M2BycgE1dOUR-ezuFKl8pwvxVLovYQn89fGoAfDn_dDpPBrbNXC-pmgR/exec`
- **Form submissions confirmed working** — rows appear in Google Sheets Submissions tab

## IMPORTANT: Old vs Correct Sheet

There are TWO Google Sheets. The OLD one (wrong) has ID `1Um_qzSusR5z5Pmh9-C8eqe07_RDk3YrM` — ignore it. The CORRECT one is `1rM1Zn_lvD9TzDImPW8Q3JmyWVhfrMEPH_20vTJOikO8`. All scripts and Power BI must use the correct ID.

## Google Sheet tabs

- **MissedRentals** — historical data (pre-existing log)
- **Submissions** — live form submissions go here
- **Crosswalk** — existing tab, purpose unknown
- **Item Rate Sheet** — pricing lookup table

## Submissions tab columns (in order)

Timestamp, StaffName, RentalDate, Item, Category, Duration, Quantity, Reason, Notes

## Item Rate Sheet columns

Description, Item Category, 4 Hours, Day, Week, 4 Week

## Apps Script (Extensions > Apps Script in Google Sheet)

Two functions exist:
1. `doPost(e)` — receives form submissions and writes to Submissions tab
2. `importMissedRentalsFast()` — imports historical data from MissedRentals tab to Submissions

The script uses `SpreadsheetApp.openById('1rM1Zn_lvD9TzDImPW8Q3JmyWVhfrMEPH_20vTJOikO8')`.

## Power BI — FULLY COMPLETE

- **Local file:** `C:\Users\Tommy\OneDrive - Dutch Rentalz\Apps\Missed Rentals app\Missed rentals.pbix`
- **Published to:** app.powerbi.com → My Workspace → "Missed rentals"
- **Data source:** Google Sheets via Web connector (anonymous auth)
- **Export URL:** `https://docs.google.com/spreadsheets/d/1rM1Zn_lvD9TzDImPW8Q3JmyWVhfrMEPH_20vTJOikO8/export?format=xlsx`

## Power Query (already applied — do not redo)

Both tables have these steps applied:
- **Submissions:** Source → Navigation → Promoted Headers → Changed Type → Removed Blank Rows → Removed Errors
- **Item Rate Sheet:** Source → Navigation → Promoted Headers → Changed Type → Removed Blank Rows → Removed Errors

## Relationship (already created)

Submissions[Item] → Item Rate Sheet[Description], many-to-one (*:1), cross filter both directions, active

## DAX calculated column (already added to Submissions table)
