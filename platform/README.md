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

- **Home**: six outcome KPI cards (Active Opportunities, Pipeline Revenue, Contracted Value,
  Operational Annual Revenue, External Capital Mobilised, CAPEX Avoided), each with a change
  since the last snapshot. Below them: the Development Pipeline with its health indicators (see below), status donut,
  and a Management Attention strip (DG decisions, overdue, delayed, needs attention) above the
  attention list. Further down: next actions, Capital & CAPEX Avoidance, and breakdowns by
  objective (with strategic alignment) and by delivery model. Click any chart element to filter
  or drill down.
- **Opportunities**: the full register, with stage tabs, search, sortable columns,
  pagination and Excel export. Click a row to see its full record and data quality flags.
- **Pipeline & Actions**: Time in Stage ageing table, decisions required from the DG, the management attention list
  (sorted earliest due first), weighted vs open pipeline, and closed opportunities.
- **Data Quality**: every rule from the workbook's *Definitions* sheet, checked for each record.
- **Add / Upload** (Investment Manager only): new-opportunity form, workbook upload
  (merge by Investment ID, or replace all), upload template, snapshots, backup and restore.

## Development pipeline health

The funnel shows only the **development pipeline** (Opportunity → Contracted). **Operational** sits in a separate
*Operating portfolio* strip. Contracted is a short, passing stage, and Operational keeps accumulating, so putting
them in one funnel made "0 Contracted → 3 Operational" look like an impossible jump.

Each stage has two RAG indicators:

| Indicator | What it measures | Green | Amber | Red |
|---|---|---|---|---|
| **Stage mix vs guide** | stage's share of the development pipeline compared with a guide shape (Opportunity 35%, Assessment 25%, Business Case 20%, Approved 12%, Contracted 8%) | within ±10 pts | ±10–20 pts | more than 20 pts off |
| **Median time in stage** | days since *Stage Entry Date*, against a per-stage limit (Opportunity 30, Assessment 45, Business Case 90, Approved 60, Contracted 90 days) | ≤ limit | ≤ 2 × limit | > 2 × limit |

The pipeline is grey ("too few to judge") when it has fewer than 5 opportunities, or when no entry dates are recorded.
The *Pipeline & Actions* page breaks each stage into ageing buckets (< 30, 30–90, 90–180, > 180 days), and the
register has an **In Stage** column.

The guide shares and time limits are **planning assumptions**, set as `GUIDE` and `AGE_LIMIT` at the top of the script.
Replace them with RAKTA's own conversion rates and stage durations after 12–18 months of gate history.

**Stage Entry Date** is a new field. It fills in automatically with today's date when the Stage is changed in the
form, and it can be edited for back-dating. Active opportunities without it get a Data Quality flag. The v4 workbook
has no such column, so it must be entered once for existing opportunities.

## Language (English / العربية)

The **ع / EN** button in the header switches the whole interface between English and Arabic, and the
choice is remembered. In Arabic the layout flips right-to-left: the sidebar moves to the right, the
drawer slides in from the left, and bars, borders and arrows mirror. A link can open straight in Arabic
with `?lang=ar`, and it combines with the role parameter, for example `?role=dg&lang=ar`.

- Interface text and list values (stages, statuses, sectors, delivery models, strategic objectives,
  units, gate decisions and so on) are translated. Data typed by users (opportunity names, next
  actions, partner names, owners) stays as entered.
- Amounts read as `243 مليون درهم`. Digits stay Western (0–9), as is usual in UAE government documents.
- Excel and CSV export and the upload template keep English column headers, so files still re-import
  whatever language they were downloaded in.
- The Arabic text sits in one dictionary (`AR.ui` for interface text, `AR.val` for list values) near the
  top of the script, so wording can be changed in one place. The authority's Arabic name and the
  strategic objective titles should be checked against RAKTA's approved Arabic terminology.

## Partners and owners

- **Partner**: the company, client or investor RAKTA would work with. This is the workbook's
  *Customer / Partner* column, plus an optional **Partner Role** (Client, Investor, JV / Co-investor,
  Operator / Concessionaire, Landowner / Enabler). The partner's logo appears after the ID in every
  table. Hover over it to see the name; click it to filter the whole dashboard by that partner.
  Partners without a logo show coloured initials.
- **Logos** are uploaded in the opportunity form or under *Add / Upload → Partner Logos*.
  Empty white or transparent margins are trimmed automatically when a logo is uploaded, so wide and tall logos fill their box. Each logo is stored at up to 480 px and shared by every opportunity with the same partner name.
  Backups include the logos.
- **Owner** is the named person to follow up with. **Owning Unit** is the department. The register
  shows the owner with the unit underneath, and a Data Quality flag appears when the owner field
  holds a department name.

## Calculation rules (from the workbook's Definitions sheet)

- Indicative Contract Revenue = Expected Annual Revenue × Term. This is revenue, not NPV or profit.
- Weighted Pipeline Revenue = Indicative Contract Revenue × stage weight (10 / 25 / 50 / 75 / 100 / 100 %).
  The weights are **planning assumptions**. Recalibrate them from actual gate conversions once history exists.
- Estimate Confidence describes the quality of the evidence behind the numbers. It is **not** a closing probability.
- Open and Weighted Pipeline count pre-contract stages only (Opportunity → Approved).
  Contracted Value counts the Contracted and Operational stages.
- Rejected and Withdrawn opportunities are excluded from every active KPI.
- Status is checked against its rule: a next action 1–14 days overdue should be
  **Attention**, and more than 14 days overdue should be **Delayed**.

## Capital and CAPEX avoidance

| Field | Input / calc | When |
|---|---|---|
| Total Project CAPEX | input | when known (v4 "Investment/CAPEX Required" maps here) |
| RAKTA CAPEX Required | input | flagged if missing from Business Case onwards |
| External Capital | Total − RAKTA | calculated |
| Counterfactual RAKTA CAPEX | input, **optional** | only if RAKTA would otherwise have funded the asset itself |
| CAPEX Avoidance Status | Potential / Validated / Realised | defaults to Potential |
| CAPEX Baseline Source | text | required when a counterfactual is entered (e.g. "2027 Approved Capital Plan") |
| CAPEX Avoided | Counterfactual − RAKTA CAPEX | calculated |

Reporting rules:
- **CAPEX Avoided** (headline) counts Validated + Realised only. Potential is shown separately and never reported as avoided.
- **External Capital Mobilised** (headline) counts Contracted + Operational only. Pre-contract capital appears as "in pipeline".
- External capital and CAPEX avoided are separate measures. A partner funding a project is not CAPEX avoidance unless RAKTA had a real, evidenced obligation to fund it.
- Data Quality flags: avoidance claimed without a source, Potential still unvalidated at Approved+, Realised before Contracted, and RAKTA CAPEX greater than Total.

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
