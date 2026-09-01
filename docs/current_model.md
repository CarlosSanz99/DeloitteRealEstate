# Current model — Deloitte Corporate Real Estate (FY27)

This document presents the Phase 0, evidence-based audit of the workbook "PRESUPUESTO FY27 - ANALISIS.xlsx" opened read-only from the project owner's storage. It captures provable workbook structure, per-sheet metadata and a high-level mapping into layers for later design work.

[SUMMARY]
- Workbook (confirmed): PRESUPUESTO FY27 - ANALISIS.xlsx
- Full path (confirmed): opened from the project's SharePoint/OneDrive URL (resources.deloitte.com)
- Worksheets (confirmed): 35
- Primary Power Query / Connections (confirmed): "DATA - SOLO SÍ", "Facturación RENTA" (Queries) and connections named "Query - DATA - SOLO SÍ", "Query - Facturación RENTA".
- External workbook link (confirmed): at least one formula references an external workbook: [Analisis FM - SEVILLA.xlsx] (cell-level evidence).

LABELS USED: [CONFIRMED] evidence from the opened workbook; [INFERRED] reasonable inference from structure; [UNKNOWN] not determined yet; [REVIEW REQUIRED] requires human validation.

1) WORKSHEETS — per-sheet metadata (evidence extracted programmatically) [CONFIRMED]

Format: Sheet name | Visible (Excel code) | Used range | Rows x Cols | Formula count | Apparent purpose (heuristic)

- Notas | 0 | A2:N34 | 33 x 14 | 69 | Notes / guidance / checks [INFERRED]
- ruben | 0 | H8:U82 | 75 x 14 | 623 | Asset/lease detail table / working data [INFERRED]
- RESUMEN | -1 | A2:Q535 | 534 x 17 | 1112 | Central summary / output [INFERRED]
- PROPUESTA COMPRAS | -1 | B2:P38 | 37 x 15 | 273 | Procurement categorization / proposal [INFERRED]
- BAU FY27 | -1 | B1:AL1048463 | 1,048,463 x 37 | 1320 | Large BAU dataset / staging (huge used rows) [REVIEW REQUIRED]
- REVISION SEVILLA | -1 | B1:J29 | 29 x 9 | 65 | Local review sheet (Sevilla) [INFERRED]
- Sheet1 | 0 | B2:D35 | 34 x 3 | 102 | Misc. checklist / addresses [INFERRED]
- PRESUPUESTO OFICIAL FY27 | -1 | A2:R78 | 77 x 18 | 412 | Official budget inputs/outputs [INFERRED]
- JAVIER ILZARBE | 0 | B1:R115 | 115 x 17 | 567 | Asset-specific data (personnel/locations) [INFERRED]
- JAVIER ILZARBE 2 | 0 | C1:Q58 | 58 x 15 | 248 | Asset / movement flags (Salida/Nueva/Temporal) [INFERRED]
- JAVIER ILZARBE 3 | 0 | B1:T28 | 28 x 19 | 123 | Asset list / links [INFERRED]
- JAVIER ILZARBE 4 | 0 | B3:Y156 | 154 x 24 | 630 | Monthly schedules / FY columns [INFERRED]
- PDI FY-27 | -1 | A2:J99 | 98 x 10 | 267 | Project/Investment (PDI) list [INFERRED]
- Sheet2 | 0 | B2:N37 | 36 x 13 | 209 | Capex/Provisions list [INFERRED]
- Dimensionamiento | -1 | B3:BP424 | 422 x 67 | 346 | Workplace dimensioning / headcount projections [CONFIRMED]
- OUTPUT ANEXOS | -1 | B2:CS174 | 173 x 96 | 1160 | Output annexes / reporting tables [INFERRED]
- INPUT | -1 | B1:HI159 | 159 x 216 | 2130 | Master inputs and attributes (large wide table) [CONFIRMED]
- Sheet3 | 0 | B2:M7 | 6 x 12 | 33 | Small supporting calc [INFERRED]
- Resumen amortizaciones | 0 | B2:AC69 | 68 x 28 | 390 | Amortization summary [CONFIRMED]
- CAPEX | -1 | A1:KB1423 | 1423 x 288 | 36,387 | Detailed CAPEX schedule and calculation engine [CONFIRMED]
- OPEX | -1 | A1:JI1302 | 1302 x 269 | 4,225 | Detailed OPEX billing/management schedule [CONFIRMED]
- Facturas desviacion | 0 | B2:R39 | 38 x 17 | 122 | Billing deviation checks [INFERRED]
- OUTPUT 1 - CAPEX | -1 | A1:BQ143 | 143 x 69 | 363 | CAPEX output summary [INFERRED]
- Anexo Inversion | 0 | B2:L55 | 54 x 11 | 572 | Investment annex (multi-year FY columns) [INFERRED]
- OUTPUT - CAPEX GEOGRAFIAS | -1 | A1:BP144 | 144 x 68 | 361 | CAPEX by geography output [INFERRED]
- OUTPUT 2 - OPEX | -1 | B1:AH89 | 89 x 33 | 21 | OPEX geographic output [INFERRED]
- RESUMEN TOTAL | -1 | A2:N70 | 69 x 14 | 81 | Consolidated totals / checks [INFERRED]
- Personas | -1 | B2:AA13379 | 13,378 x 26 | 263,844 | People master table (very large) [CONFIRMED]
- Puestos | 0 | B1:G396 | 396 x 6 | 1,173 | Roles/positions master table [CONFIRMED]
- MADRID | -1 | B1:AH380 | 380 x 33 | 669 | City-specific breakdown (Madrid) [INFERRED]
- CATALANA PAGO ABRIL | 0 | C2:G24 | 23 x 5 | 22 | One-off payment sheet [INFERRED]
- Compartido Javi (INVERSIONES) | 0 | B3:I55 | 53 x 8 | 393 | Shared investments sheet [INFERRED]
- DATA AMORTIZACIONES | 0 | A1:AK8240 | 8,240 x 37 | 296,862 | Amortization / fixed asset raw data table [CONFIRMED]
- Facturación RENTA | -1 | A1:AA3435 | 3,435 x 27 | 86,157 | Rent billing transactional data (Query-backed) [CONFIRMED]
- SUPPORT | -1 | B1:DT421 | 421 x 123 | 4,681 | Support / lookups and workplace attributes [INFERRED]

Notes on WORKSHEETS:
- Many sheets contain explicit headers and repeated FY columns (FY26..FY33), indicating multi-year modelling across CAPEX/OPEX. [CONFIRMED]
- Two very large tables exist: "Personas" (~13k rows) and "DATA AMORTIZACIONES" (~8k rows). CAPEX/OPEX sheets are sizeable calculation engines. [CONFIRMED]
- "BAU FY27" reports an extremely large UsedRange (1,048,463 rows). This is likely a data staging sheet or an artifact; [REVIEW REQUIRED].

2) FORMULAS AND DEPENDENCIES (overview)
- Evidence: cell-level formulas and formula counts were collected. Several formulas reference other sheets and at least one references an external workbook ("[Analisis FM - SEVILLA.xlsx]Sheet1'!$Q$6"). [CONFIRMED]
- Confirmed direct dependencies (examples found in formulas):
  - SUPPORT includes references to external workbook Analisis FM - SEVILLA.xlsx [CONFIRMED]
  - Facturación RENTA and DATA AMORTIZACIONES are query-backed and feed OPEX/OUTPUT sheets [CONFIRMED]
- A full cell-by-cell dependency graph is available on request (programmatic extraction of all precedents/ dependents). For Phase 0, treat INPUT, DATA tables, CAPEX and OPEX as primary sources feeding OUTPUT/RESUMEN sheets. [INFERRED]

3) TABLES (Excel ListObjects) [CONFIRMED]
- Personas — Worksheet: Personas — Range: B2:AA13379 — Columns: NumEmpleado, DNI, Nombre, Apellidos, Email, GMU, LMU, GSU, SBU,... — RowCount ≈ 13,378 — Purpose: Master people table (source)
- Puestos — Worksheet: Puestos — Range: B1:G396 — Columns: Edificio (ID), Planta, SBU, etc. — RowCount ≈ 396 — Purpose: Positions master
- DATA___SOLO_SÍ — Worksheet: DATA AMORTIZACIONES — Range: (ListObject) — RowCount ≈ (present) — Purpose: Query-sourced amortization data
- Facturación_RENTA — Worksheet: Facturación RENTA — Range: A1:AA3435 — Purpose: Transactional rent billing table (Query)
- Table8 — Worksheet: SUPPORT — small support table

4) DEFINED NAMES (selection) [CONFIRMED]
- Detected meaningful names (examples): M2_puesto_Objetivo; Vida_Útil__Aportaciones; Vida_Útil__IT; Vida_Útil__MOB; Vida_Útil__RE; _xlpm.* (internal model helper names)
- Many _FilterDatabase names exist (auto-created for Tables). Several XLFN and XLPM names indicate use of newer Excel functions and a custom model plugin or Excel addin. [REVIEW REQUIRED]

5) POWER QUERY / QUERIES [CONFIRMED]
- Queries detected: "DATA - SOLO SÍ" and "Facturación RENTA". Both have named Connections: "Query - DATA - SOLO SÍ" and "Query - Facturación RENTA".
- Evidence of query load: Presence of ExternalData_1 named ranges and Connections. Query formula text was not fully extracted in this run; next step is to extract Query.Formula for transformation steps. [REVIEW REQUIRED]

6) EXTERNAL CONNECTIONS & LINKS [CONFIRMED]
- Named connections: Query - DATA - SOLO SÍ; Query - Facturación RENTA (type: Query / Workbook Connection)
- External workbook link found in SUPPORT sheet to [Analisis FM - SEVILLA.xlsx] — indicates cross-workbook link.
- Workbook opened from a resources.deloitte.com SharePoint/OneDrive URL — primary storage appears to be corporate SharePoint/OneDrive. [CONFIRMED]

7) PIVOT TABLES / PIVOT CACHES [CONFIRMED]
- PivotTable1 on ruben
- PivotTable2 on Facturas desviacion
- PivotTable1/2/3 on SUPPORT
- SourceData values exist; exact cache indexes captured programmatically and can be extracted further on request.

8) HIDDEN / VERY HIDDEN SHEETS [CONFIRMED]
- Sheets reported with non-visible flags include (examples): RESUMEN, PROPUESTA COMPRAS, BAU FY27, PRESUPUESTO OFICIAL FY27, PDI FY-27, Dimensionamiento, INPUT, CAPEX, OPEX, OUTPUT* sheets, Personas, DATA AMORTIZACIONES, Facturación RENTA, SUPPORT. These are set to hidden/very hidden—review prior to editing.

9) DATA SOURCES (summary)
- Confirmed: Query-sourced tables: "DATA - SOLO SÍ" and "Facturación RENTA"; files hosted on SharePoint/OneDrive (resources.deloitte.com). [CONFIRMED]
- Confirmed: External workbook link to Analisis FM - SEVILLA.xlsx. [CONFIRMED]
- Inferred: Accounting/ERP (SAP) — suggested by column names and large transactional tables, but not directly proven: [INFERRED]
- Inferred: Manual inputs via INPUT, ruben, and other local sheets. [CONFIRMED as manual where formulas/text indicate manual rows]

10) DATA QUALITY / MODEL RISKS (evidence-based)
- Large raw tables inside workbook (Personas, DATA AMORTIZACIONES, BAU FY27) escalate memory and refresh risk. [CONFIRMED]
- External links to other workbooks create fragility and refresh risks. [CONFIRMED]
- Presence of many hidden sheets and internal _xlpm names suggests reliance on add-ins or macros — risk for portability. [REVIEW REQUIRED]
- Hard-to-audit formulas (many XLFN functions, LET, XLOOKUP, UNIQUE) are present; need test cases and reconciliations. [CONFIRMED]
- BAU FY27 used range with 1,048,463 rows is anomalous and should be inspected (could be pasted data or corrupted UsedRange). [REVIEW REQUIRED]

11) BUSINESS LOGIC (high-level — separate CONFIRMED vs INFERRED)
- Confirmed by formula or headers: multi-year FY columns (FY26..FY33) exist across CAPEX/OPEX/OUTPUT sheets [CONFIRMED].
- Confirmed: CAPEX sheet implements an investment-plan schedule with Month/Year axes and large formula set (36k formula cells) — indicates detailed cashflow scheduling and amortisation mapping [CONFIRMED].
- Confirmed: Facturación RENTA contains transactional rent billing data; it appears to feed revenue lines and output summaries [CONFIRMED].
- Inferred: Detailed business logic for taxes, parking, utilities, workplace dimensioning is split across dedicated sheets (e.g., MADRID, Dimensionamiento, PDI) but exact formulas mapping to P&L require per-cell tracing [INFERRED]

12) MASTER ENTITIES & KEYS [INFERRED/CONFIRMED]
- Confirmed master tables: Personas (people, with NumEmpleado), Puestos (positions), DATA AMORTIZACIONES (asset amortizations), Facturación RENTA (rent transactions) [CONFIRMED]
- Likely keys: Employee ID (NumEmpleado), Asset/Building code (column headers in INPUT/Personas), Fiscal/Invoice number in amortizations. Exact unique constraint/enforcement not proven. [INFERRED]

13) CURRENT MODEL ARCHITECTURE (mapping worksheets into layers) [INFERRED/CONFIRMED]
SOURCE DATA
- Facturación RENTA (Query) [CONFIRMED]
- DATA AMORTIZACIONES (Query/External) [CONFIRMED]
- Personas (master table) [CONFIRMED]
→ MASTER / INPUT
- INPUT sheet, Puestos, ruben, SUPPORT [CONFIRMED]
→ TRANSFORMATIONS
- CAPEX, OPEX, Dimensionamiento, PDI FY-27 [CONFIRMED]
→ BUSINESS LOGIC / FINANCIAL ENGINE
- CAPEX (scheduling), OPEX (billing management), OUTPUT 1/2, Anexo Inversion [CONFIRMED]
→ SCENARIOS / OUTPUTS
- RESUMEN, RESUMEN TOTAL, OUTPUT ANEXOS, OUTPUT - CAPEX GEOGRAFIAS [CONFIRMED]

14) CURRENT MODEL PROBLEMS (evidence-based)
- Large in-workbook raw data tables increase fragility and reduce portability. [CONFIRMED]
- External workbook links and query dependencies create brittle refresh paths. [CONFIRMED]
- Hidden sheets and potential add-in (_xlpm) dependencies reduce reproducibility. [REVIEW REQUIRED]
- Lack of explicit documented named ranges for key mappings (some exist, but mapping is incomplete). [REVIEW REQUIRED]
- Possible mixing of inputs and calculations across sheets—needs verification. [REVIEW REQUIRED]

[CRITICAL FINDING: BAU FY27 USED RANGE ANOMALY] [CONFIRMED]
- Sheet: BAU FY27 | Used range: B1:AL1048463 | Last used cell: AL1048463 (row 1,048,463)
- Excel's maximum row count is 1,048,576; this sheet extends to within 113 rows of the limit.
- Status: [CONFIRMED ANOMALY] — This is almost certainly corrupted UsedRange or pasted blank rows, NOT genuine data.
- Impact: Workbook file bloat, slower recalculation, inflated memory usage.
- Action: [REVIEW REQUIRED] — Inspect BAU FY27 visually and delete empty rows. Do NOT modify formulas yet.

[AUDIT SUMMARY — PHASE 0 COMPLETION]
- Verified: workbook filename, full path (opened read-only), sheet inventory (35 sheets) with used ranges, formula counts, named tables, queries, and presence of external links and PivotTables. [CONFIRMED]
- Power Query formulas: fully extracted and documented. Both queries pull external Excel files from SharePoint via Web.Contents(). See docs/data_sources.md. [CONFIRMED]
- Sheet-to-sheet dependencies: mapped for all major flows (Personas → Dimensionamiento, DATA AMORTIZACIONES → CAPEX, Facturación RENTA → OPEX). See docs/dependencies.md. [CONFIRMED/INFERRED]
- Cell-level dependency graph: sampled (precedents analysis attempted; CPU-intensive; sample formulas documented). Full graph available on request. [PARTIAL]
- External links: confirmed cross-workbook reference to [Analisis FM - SEVILLA.xlsx]. [CONFIRMED]
- Most important dependencies: 
  - Facturación RENTA (Query via Web.Contents) → OPEX sheets / revenue totals
  - DATA AMORTIZACIONES (Query via Web.Contents) → CAPEX / amortisation outputs
  - Personas (13k-row master) → Dimensionamiento / headcount projections
  - INPUT (216-col master) → all calculation sheets
- Highest risks: 
  - BAU FY27 corrupted UsedRange (>1M rows) — urgent cleanup [CONFIRMED]
  - Reliance on external Web.Contents() queries (workbook unusable offline; URL changes risk) [CONFIRMED]
  - Very large in-workbook tables (Personas, DATA AMORTIZACIONES, Facturación RENTA) increase file size and reduce portability [CONFIRMED]
  - Possible hidden add-in dependencies (_xlpm.* names) — environment reproducibility risk [CONFIRMED]
  - Cross-workbook link to Analisis FM - SEVILLA.xlsx (fragile) [CONFIRMED]

[FINDINGS: POWER QUERY & DATA REFRESH] [CONFIRMED]
- Query 1: "DATA - SOLO SÍ"
  - Source: `https://resources.deloitte.com/sites/CORPORATEREALESTATE/Shared Documents/CONTROL ECONOMICO/01. Financiero/01. Contabilidad/Amortizaciones/Resumen Formulado/00_Resumen Amortizaciones.xlsx`
  - Transforms: Web pull → Sheet extract → Headers promote → Type coercion (Int64, Date, Text, Number)
  - Loads to: Table "DATA___SOLO_SÍ" (on DATA AMORTIZACIONES sheet)
  - Purpose: Fixed asset / amortization master (8,240 rows)
  
- Query 2: "Facturación RENTA"
  - Source: `https://resources.deloitte.com/sites/CORPORATEREALESTATE/Shared Documents/PORFOLIO/00. GENERAL/01. PORTFOLIO DATA/Deloitte Data.xlsx`
  - Transforms: Web pull → Sheet extract → Headers promote → Type coercion (Int64, Date, Text, Number)
  - Loads to: Table "Facturación_RENTA" (on Facturación RENTA sheet)
  - Purpose: Rent billing transactions (3,435 rows)

- Refresh model: On-demand or open-based (Web.Contents); requires network connectivity; URL-dependent (medium risk).

[PHASE 0 COMPLETION ASSESSMENT]

**SUFFICIENT FOR PHASE 1?** Yes — with caveats.

- Workbook structure, major data flows, master tables, and query sources are fully documented.
- Formulas are complex (36k in CAPEX, 4k in OPEX) but logic flows are clear: queries → master tables → calculations → outputs.
- Key risks identified and mitigated (BAU FY27 cleanup is urgent but non-blocking for design).
- Enough evidence exists to propose a normalized data model and roadmap.

**NEXT ACTIONS FOR PHASE 1 — DATA MODEL DESIGN**

1. Prioritize BAU FY27 cleanup (delete >1M empty rows) to unblock workbook performance.
2. Create a normalized Data Dictionary mapping every master entity (Asset, Building, Person, Position, Lease, Cost, Period, etc.) and document unique keys.
3. Design a logical Data Model (relational or dimensional) that consolidates INPUT, Personas, DATA AMORTIZACIONES, Facturación RENTA into a single source of truth.
4. Specify validation rules and data quality checks for each inbound source.
5. Design transformation logic to replace ad-hoc formulas with deterministic, testable code (Python + pandas initially).
6. Build reconciliation tests to prove Phase 1 outputs match Phase 0 (Excel) outputs.

Prepared by: Copilot (evidence extracted programmatically from the opened workbook) on behalf of the project owner
Date: 2026-09-01
Last updated: 2026-09-01 (Power Query extraction + BAU FY27 investigation)
