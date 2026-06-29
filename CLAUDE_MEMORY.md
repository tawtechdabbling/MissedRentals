# Missed Rentals — Claude Session Memory

Use this file to restore context in a new Claude session. Paste the contents into the chat or say "read the memory file from the MissedRentals GitHub repo."

---

## What's built and working

- **Form:** Custom HTML form at `https://tawtechdabbling.github.io/MissedRentals/` — fully functional, no login required for staff
- **Form file (local):** `/Users/thomaswagler/Library/CloudStorage/OneDrive-DutchRentalz/Apps/Missed Rentals app/index.html`
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

## Power BI status

- Power BI Desktop installed on Windows
- .pbix file exists
- **In progress:** Reconnecting to correct Google Sheet from scratch
- Power BI Export URL to use: `https://docs.google.com/spreadsheets/d/1rM1Zn_lvD9TzDImPW8Q3JmyWVhfrMEPH_20vTJOikO8/export?format=xlsx`
- Old data sources were deleted; new ones being added via Get Data → Web
- Navigator window opened successfully with Submissions and Item Rate Sheet visible
- Authentication: Anonymous (works because sheet is set to Anyone with link can view)
- **Stopped at:** User clicked Transform Data in Navigator — need to continue from Power Query

## DAX calculated column for Missed Revenue

In Submissions table, Data view, New Column:

```
Missed Revenue =
VAR rate =
    SWITCH(Submissions[Duration],
        "4 Hours", RELATED('Item Rate Sheet'[4 Hours]),
        "Day",     RELATED('Item Rate Sheet'[Day]),
        "Week",    RELATED('Item Rate Sheet'[Week]),
        "4 Weeks", RELATED('Item Rate Sheet'[4 Week])
    )
RETURN rate * Submissions[Quantity]
```

Note: Item Rate Sheet column names are: `4 Hours`, `Day`, `Week`, `4 Week` (singular, mixed case)

## Relationship needed in Power BI

Submissions[Item] → Item Rate Sheet[Description], many-to-one, cross filter both directions

## Power Query steps needed (apply each time data is loaded)

For both Submissions and Item Rate Sheet:
- Remove Blank Rows (Home → Remove Rows → Remove Blank Rows) on key columns
- Remove Errors (Home → Remove Rows → Remove Errors) on all columns

## Visuals to build

Tommy wants:
1. Total missed revenue (Card visual)
2. Missed revenue by item, broken down by month, quarter, and year (Matrix visual)
3. Missed revenue by reason (Clustered bar chart)

## Next steps

1. Complete Power Query setup for the two new tables (remove blanks/errors)
2. Close & Apply in Power Query
3. Set up relationship: Submissions[Item] → Item Rate Sheet[Description]
4. Add Missed Revenue DAX calculated column
5. Build the three visuals above
6. Publish to Power BI web (app.powerbi.com) so Tommy can view on any browser/device

## Key decisions made

- Staff have no Microsoft accounts — form must be publicly accessible (no login)
- Rejected: Power Automate, SharePoint, Make.com
- Chose: Google Sheets as data store, GitHub Pages for form hosting
- No price field on form — missed revenue calculated in Power BI via DAX
- Historical import was attempted but caused issues — Tommy decided to skip for now and start fresh with live submissions only
