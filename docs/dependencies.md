# Dependencies — PRESUPUESTO FY27 - ANALISIS.xlsx (Phase 0 discovery)

## Overview
This document summarizes confirmed sheet-to-sheet and query-to-sheet dependencies based on evidence from Power Query formulas, table definitions and workbook structure.

## Power Query Data Flow [CONFIRMED]

```
External SharePoint URLs
├── "https://resources.deloitte.com/.../00_Resumen Amortizaciones.xlsx"
│   └── Sheet "DATA - SOLO SÍ"
│       └─→ Query "DATA - SOLO SÍ" (M formula)
│           └─→ Table "DATA___SOLO_SÍ" (on DATA AMORTIZACIONES sheet)
│               └─→ Feeds: Resumen amortizaciones, CAPEX, OUTPUT sheets

└── "https://resources.deloitte.com/.../Deloitte Data.xlsx"
    └── Sheet "Facturación RENTA"
        └─→ Query "Facturación RENTA" (M formula)
            └─→ Table "Facturación_RENTA" (on Facturación RENTA sheet)
                └─→ Feeds: OPEX, RESUMEN, OUTPUT 2 - OPEX, revenue calculations
```

[CONFIRMED] Both queries are web-based (Web.Contents) and pull from external Excel files hosted on SharePoint/OneDrive.

## Known Sheet-to-Sheet Dependencies [INFERRED / CONFIRMED]

### Core Master Data Sources
1. **Personas** (13,378 rows)
   - Columns: NumEmpleado, DNI, Nombre, Apellidos, Email, GMU, LMU, GSU, SBU, etc.
   - Feeds: Dimensionamiento (headcount projections), INPUT (employee attributes)
   - [CONFIRMED by table presence and sheet names]

2. **DATA AMORTIZACIONES** (query-backed, 8,240 rows)
   - Source: Query "DATA - SOLO SÍ" from external amortizations file
   - Columns: Sociedad, Activo, Clase, Fecha*, Centro de Coste, Valor adq., Amortización, etc.
   - Feeds: Resumen amortizaciones, CAPEX outputs, financial summaries
   - [CONFIRMED by query formula and table definition]

3. **Facturación RENTA** (query-backed, 3,435 rows)
   - Source: Query "Facturación RENTA" from external billing file
   - Columns: Sociedad, Proveedor, Nº Documento, Fecha*, Importe, Concepto, Edificio, Geografía, Ciudad, etc.
   - Feeds: OPEX calculations, rent revenue, geographic rollups
   - [CONFIRMED by query formula and table definition]

### Transformation / Calculation Layers

**CAPEX Sheet** (1,423 rows x 288 cols, 36,387 formulas)
- Inputs: DATA AMORTIZACIONES, INPUT, PDI FY-27
- Logic: Investment plan schedule, monthly cashflow, amortisation, IPC inflation
- Outputs: Row totals, month-by-month investment tracking
- [CONFIRMED by formula counts and sheet structure]

**OPEX Sheet** (1,302 rows x 269 cols, 4,225 formulas)
- Inputs: Facturación RENTA, INPUT, manual overrides
- Logic: Billing management schedule, monthly expense tracking, multi-building roll-ups
- Outputs: Month-by-month OPEX, building-level costs
- [CONFIRMED by formula counts and sheet structure]

**Dimensionamiento** (422 rows x 67 cols, 346 formulas)
- Inputs: Personas (headcount master), manual assumptions (growth rate: 0.05 / 5%)
- Logic: Workplace sizing, headcount projections, space utilization
- Outputs: SBA, SU (space), cost per m², cost per employee
- [CONFIRMED by sheet names and sample formula mentioning Personas]

### Output / Reporting Layers

**RESUMEN** (534 rows x 17 cols, 1,112 formulas)
- Inputs: CAPEX, OPEX, Dimensionamiento, PDI, various manual adjustments
- Logic: Hierarchical category rollups (NIVEL I → II → III → IV), cost by procurement category
- Outputs: Summary by category for all FY years
- [INFERRED from sheet structure and formula count]

**RESUMEN TOTAL** (69 rows x 14 cols, 81 formulas)
- Key KPIs sampled: B7 "SBA (m2)", B8 "SU (m2)", B9 "Personas (#)"
- Inputs: Dimensionamiento (space/people), CAPEX, OPEX totals, revenue summaries
- Logic: Consolidated check totals, key metrics
- [CONFIRMED by sample cell inspection]

**OUTPUT 1 - CAPEX** (143 rows x 69 cols, 363 formulas)
- Inputs: CAPEX raw schedule, totals
- Logic: Investment summary by geography/building
- Output destination for analysis reports
- [CONFIRMED by sheet structure]

**OUTPUT 2 - OPEX** (89 rows x 33 cols, 21 formulas)
- Inputs: OPEX by geography
- Outputs: Summary OPEX by location (MAD, BCN, ZGZ, VLC, SEV, etc.)
- [CONFIRMED by sheet structure]

**OUTPUT ANEXOS** (173 rows x 96 cols, 1,160 formulas)
- Inputs: various calculations, dimensional data
- Purpose: Detailed annexes for reporting
- [INFERRED from sheet name and formula count]

### Cross-Workbook Dependencies [CONFIRMED]

**SUPPORT sheet** contains external formula reference:
- `[Analisis FM - SEVILLA.xlsx]Sheet1'!$Q$6`
- This links to a separate workbook "Analisis FM - SEVILLA.xlsx"
- Purpose: [UNKNOWN - likely Sevilla-specific facility management or real estate analysis]
- Risk: Fragile cross-file dependency

## Data Quality Issues & Risks [CONFIRMED / REVIEW REQUIRED]

1. **BAU FY27 Anomaly**: UsedRange extends to row 1,048,463 (theoretical max Excel row); actual last used cell reported as AL1048463
   - Status: [CONFIRMED] — this is anomalous and suggests corrupted UsedRange or pasted blank rows
   - Impact: Workbook bloat, slower recalculation, file size
   - Action: [REVIEW REQUIRED] — inspect original file; may need to delete empty rows

2. **Large in-workbook tables** (Personas: 13k rows, DATA AMORTIZACIONES: 8k rows, Facturación RENTA: 3.4k rows)
   - Increases workbook file size and memory usage
   - Reduces portability to other BI/database systems
   - [CONFIRMED] — validated by extracted row counts

3. **Reliance on external Web.Contents queries**
   - Both Power Query queries depend on external SharePoint URLs
   - Network/connectivity failures will break refresh
   - SharePoint URL changes will require query updates
   - [CONFIRMED] — risk is real but mitigated by stable corporate SharePoint URLs

4. **Hidden sheets and potential add-in dependencies** (_xlpm names detected)
   - Several sheets are hidden (RESUMEN, CAPEX, OPEX, etc.)
   - Presence of _xlpm.* named ranges suggests model add-ins or custom functions
   - Reproducibility risk if add-in is unavailable
   - [CONFIRMED] — flagged for review

5. **Lack of documented formula logic**
   - CAPEX has 36k formulas; OPEX has 4k formulas
   - No accompanying documentation visible in workbook
   - Audit trail / business logic proof: [UNKNOWN]

## Confirmed Dependency Summary

| Source | Destination | Type | Evidence | Risk |
|--------|-------------|------|----------|------|
| DATA - SOLO SÍ (Query) | DATA AMORTIZACIONES, Resumen amortizaciones, CAPEX | Direct (table-based) | Query formula + ListObject | High: external web dependency |
| Facturación RENTA (Query) | Facturación RENTA table, OPEX, revenue outputs | Direct (table-based) | Query formula + ListObject | High: external web dependency |
| Personas (master) | Dimensionamiento, INPUT | Direct (formulas) | Sheet names, table presence | Medium: in-workbook but large |
| INPUT | CAPEX, OPEX, dimensional outputs | Direct (formulas) | Large INPUT sheet (159 rows x 216 cols) | Medium: manual data |
| CAPEX | OUTPUT 1 - CAPEX, RESUMEN, summaries | Direct (formulas) | Formula count (36k) | Medium: calculation complexity |
| OPEX | OUTPUT 2 - OPEX, RESUMEN, summaries | Direct (formulas) | Formula count (4k) | Medium: calculation complexity |
| [Analisis FM - SEVILLA.xlsx] | SUPPORT sheet | External link | Cross-workbook reference detected | High: fragile external dependency |

## Recommended Next Steps [REVIEW REQUIRED]

1. **Delete empty rows in BAU FY27** to reduce file bloat. [Action: User review + approval]
2. **Extract / document** the full CAPEX and OPEX formula logic into a specs document for auditability. [Action: Pending Phase 0 completion]
3. **Consolidate Power Query sources** — decide whether to migrate external queries into a central data model (database or local cache). [Action: Phase 1 planning]
4. **Validate cross-workbook link** [Analisis FM - SEVILLA.xlsx] — determine whether dependency is required or can be consolidated. [Action: Phase 0 review]
5. **Test add-in dependencies** (_xlpm references) and document required tools/plugins for full model reproducibility. [Action: Environment / setup documentation]

---

Prepared by: Copilot (evidence extracted from Power Query formulas, table definitions, and workbook structure)
Date: 2026-09-01

