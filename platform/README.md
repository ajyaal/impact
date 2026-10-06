# RAKTA Investment Portfolio Dashboard

`investment-dashboard.html` is a single self-contained page built from the
*RAKTA MVP Investment Plan & Dashboard v4* workbook and the DG dashboard mockup.
Open it in any modern browser. No build step or server is needed.

## Who uses it

| Role (header selector) | Can do |
|---|---|
| **Investment Manager** | Add, edit and delete opportunities, upload the Investment Plan workbook (.xlsx/.csv), record monthly snapshots, back up and restore data |
| **Strategy Director** | View only: every dashboard, filter, drill-down and export |
| **Director General** | View only: same as the Strategy Director |

You can open a link straight into a role with `?role=dg`, `?role=sd` or `?role=im`.
The role selector controls what the screen shows. It is **not** access control.

## Views

- **Home**: eight KPI cards (Active, Open Pipeline, Weighted Pipeline, Contracted Value,
  Investment Required, Overdue Actions, DG Decisions Pending, Strategic Alignment), each with
  a change since the last snapshot. Below them: stage funnel, value by sector, status donut,
  management attention list, next actions by timing, strategic alignment, and value by
  objective and by delivery model. Click any chart element to filter or drill down.
- **Opportunities**: the full register, with stage tabs, search, sortable columns,
  pagination and Excel export. Click a row to see its full record and data quality flags.
- **Pipeline & Actions**: decisions required from the DG, the management attention list
  (sorted earliest due first), weighted vs open pipeline, and closed opportunities.
- **Data Quality**: every rule from the workbook's *Definitions* sheet, checked for each record.
- **Add / Upload** (Investment Manager only): new-opportunity form, workbook upload
  (merge by Investment ID, or replace all), upload template, snapshots, backup and restore.

## Calculation rules (from the workbook's Definitions sheet)

- Lifetime Value = Expected Annual Revenue × Term.
- Weighted Value = Lifetime Value × stage probability (10 / 25 / 50 / 75 / 100 / 100 %).
- Open and Weighted Pipeline count pre-contract stages only (Opportunity → Approved).
  Contracted Value counts the Contracted and Operational stages.
- Rejected and Withdrawn opportunities are excluded from every active KPI.
- Status is checked against its rule: a next action 1–14 days overdue should be
  **Attention**, and more than 14 days overdue should be **Delayed**.

## Data storage

Data is saved in the browser's `localStorage`, so each browser keeps its own copy.
To share one live dataset between the Investment Manager, Strategy Director and DG, do one of these:
1. The Investment Manager uploads the current workbook and sends the backup JSON,
   or simply sends the workbook for the others to upload; or
2. Host the page behind a small shared store (SharePoint list, Dataverse or an API).
   The data layer is the `store` object plus the `DATA` array in the script.

The Excel upload/export uses SheetJS from cdnjs. When that library can't load (for
example, offline), save the workbook as CSV and upload that instead; CSV works without it.

## Source data notes (v4 workbook)

- The *Last Updated* column holds progress commentary rather than dates. The dashboard
  imports that text as **Progress Note** and flags *Last Updated* as missing.
- Only INV-021 has a due date, so 23 opportunities appear under "No due date set".
- No opportunity is flagged *DG Decision Required = Y* yet, and none has a Contracted Value.
