# Data Dictionary — Deloitte Corporate Real Estate (FY27)

This document defines the main entities, their attributes, keys, and relationships based on Phase 0 evidence extracted from PRESUPUESTO FY27 - ANALISIS.xlsx.

**Labels:**
- `[CONFIRMED]` = Evidence found in workbook metadata, tables, or formulas
- `[INFERRED]` = Reasonable inference from structure/naming, not directly proven
- `[UNKNOWN]` = Not yet determined
- `[REVIEW REQUIRED]` = Requires business/project owner validation

---

## 1. ASSET / BUILDING

**Entity name:** DimAsset (target) / INPUT + SUPPORT sheets (current)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| BuildingCode | Unique building/asset identifier | String | INPUT, SUPPORT | Yes (Primary) | - | [INFERRED] |
| BuildingName | Official building name | String | INPUT, SUPPORT | No | - | [INFERRED] |
| City | City/location | String | SUPPORT, OUTPUT sheets | No | FK→DimLocation | [CONFIRMED] |
| Geography | Geographic region/cluster | String | Facturación RENTA, OUTPUT sheets | No | FK→DimLocation | [CONFIRMED] |
| Address | Physical address | String | SUPPORT | No | - | [INFERRED] |
| SBA | Superfície Bruta Alquilada (gross leased area) m² | Decimal | SUPPORT, Dimensionamiento | No | - | [CONFIRMED] |
| SU | Superficie Útil (usable area) m² | Decimal | SUPPORT, Dimensionamiento | No | - | [CONFIRMED] |
| CostCenter | Deloitte cost center code | String | DATA AMORTIZACIONES, Facturación RENTA | No | - | [CONFIRMED] |
| Sociedad | Legal entity / company code | String | DATA AMORTIZACIONES, Facturación RENTA | No | FK→DimOrganization | [CONFIRMED] |
| Status | Active, closed, temporary, planned | String | JAVIER ILZARBE sheets | No | - | [INFERRED] |
| OpenDate | Date building opened/occupied | Date | JAVIER ILZARBE sheets | No | - | [INFERRED] |
| CloseDate | Date building closed/vacated | Date | JAVIER ILZARBE sheets | No | - | [INFERRED] |

[CONFIRMED] Major cities in dataset: Madrid, Barcelona, Zaragoza, Valencia, Sevilla (evidenced in OUTPUT sheets)

---

## 2. LOCATION / GEOGRAPHY

**Entity name:** DimLocation (target) / SUPPORT, OUTPUT sheets (current)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| GeographyCode | Region/cluster code | String | Facturación RENTA, OUTPUT sheets | Yes (Primary) | - | [CONFIRMED] |
| GeographyName | Region/cluster name | String | OUTPUT sheets | No | - | [INFERRED] |
| CityName | City | String | SUPPORT, Facturación RENTA | No | - | [CONFIRMED] |
| CityCode | City code (if exists) | String | Facturación RENTA | No | - | [UNKNOWN] |
| CountryCode | Country (assume Spain) | String | Manual | No | - | [INFERRED] |
| RegionName | Autonomous Community | String | Manual | No | - | [INFERRED] |

[CONFIRMED] Confirmed geographies in data: MAD (Madrid), BCN (Barcelona), ZGZ (Zaragoza), VLC (Valencia), SEV (Sevilla)

---

## 3. PERSON / EMPLOYEE / HEADCOUNT

**Entity name:** DimPerson (target) / Personas sheet (current, 13,378 rows)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| EmployeeID (NumEmpleado) | Unique employee identifier | String/Int | Personas | Yes (Primary) | - | [CONFIRMED] |
| DNI | National ID / Tax ID | String | Personas | No (if PII tracked separately) | - | [CONFIRMED] |
| Nombre | First name | String | Personas | No | - | [CONFIRMED] |
| Apellidos | Last name(s) | String | Personas | No | - | [CONFIRMED] |
| Email | Corporate email | String | Personas | No | - | [CONFIRMED] |
| EmploymentStatus | Active, inactive, on-leave | String | Personas | No | - | [INFERRED] |
| StartDate | Employment start date | Date | Personas | No | - | [UNKNOWN] |
| EndDate | Employment end date (if terminated) | Date | Personas | No | - | [UNKNOWN] |
| GMU | Management unit / division | String | Personas | No | FK→DimOrganization | [CONFIRMED] |
| LMU | Lower management unit | String | Personas | No | FK→DimOrganization | [CONFIRMED] |
| GSU | Geographic strategic unit | String | Personas | No | FK→DimLocation | [CONFIRMED] |
| SBU | Strategic business unit | String | Personas, INPUT | No | FK→DimOrganization | [CONFIRMED] |
| PositionID | Role/position code | String | Personas, Puestos | No | FK→DimPosition | [CONFIRMED] |
| WorkLocationCode | Assigned building/location | String | Personas, SUPPORT | No | FK→DimAsset | [INFERRED] |

[CONFIRMED] 13,378 employee rows in Personas master table as of snapshot date

---

## 4. POSITION / ROLE

**Entity name:** DimPosition (target) / Puestos sheet (current, 396 rows)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| PositionID | Unique position code | String | Puestos | Yes (Primary) | - | [CONFIRMED] |
| PositionName | Job title/role name | String | Puestos | No | - | [INFERRED] |
| Building | Building where position is based | String | Puestos | No | FK→DimAsset | [CONFIRMED] |
| Floor | Floor number | String | Puestos | No | - | [CONFIRMED] |
| SBU | Strategic business unit | String | Puestos | No | FK→DimOrganization | [CONFIRMED] |
| Grade | Job grade/level | String | Puestos | No | - | [INFERRED] |
| ManagerID | Direct manager EmployeeID | String | Puestos | No | FK→DimPerson | [UNKNOWN] |

[CONFIRMED] 396 distinct position codes in Puestos table

---

## 5. ORGANIZATION / SBU

**Entity name:** DimOrganization (target) / Personas, INPUT, Puestos (current)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| SBUCode | Strategic business unit code | String | Personas, Puestos | Yes (Primary) | - | [CONFIRMED] |
| SBUName | SBU name | String | Personas | No | - | [INFERRED] |
| GMUCode | Global management unit | String | Personas | No | - | [CONFIRMED] |
| GMUName | GMU description | String | Personas | No | - | [INFERRED] |
| LMUCode | Lower management unit | String | Personas | No | - | [CONFIRMED] |
| LMUName | LMU description | String | Personas | No | - | [INFERRED] |
| Sociedad | Legal entity code | String | DATA AMORTIZACIONES, Facturación RENTA | No | - | [CONFIRMED] |
| CostCenter | Cost center allocation | String | DATA AMORTIZACIONES | No | - | [CONFIRMED] |

[CONFIRMED] Organizational hierarchy: Sociedad → GMU → LMU → SBU confirmed in Personas and transaction data

---

## 6. CONTRACT / LEASE

**Entity name:** DimLease (target) / Facturación RENTA + SUPPORT (current)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| LeaseID | Unique lease identifier | String | Facturación RENTA | Yes (Primary) | - | [INFERRED] |
| BuildingCode | Asset/building code | String | Facturación RENTA | No | FK→DimAsset | [CONFIRMED] |
| LandlordName | Property owner/landlord | String | Facturación RENTA (Proveedor) | No | - | [CONFIRMED] |
| LandlordID / CIF | Tax/supplier ID | String | Facturación RENTA | No | - | [CONFIRMED] |
| StartDate | Lease start date | Date | SUPPORT | No | - | [UNKNOWN] |
| EndDate | Lease expiration date | Date | SUPPORT | No | - | [UNKNOWN] |
| RentAmount | Monthly/annual rent | Decimal | Facturación RENTA | No | - | [CONFIRMED] |
| Currency | Lease currency (EUR assumed) | String | Facturación RENTA (Moneda) | No | - | [CONFIRMED] |
| LeaseType | Operating lease, capital lease, etc. | String | SUPPORT | No | - | [UNKNOWN] |
| TaxTreatment | Impuesto (tax indicator) | String | Facturación RENTA | No | - | [CONFIRMED] |

[CONFIRMED] Facturación RENTA contains 3,435 transactional rent billing rows; invoice dates and amounts confirmed

---

## 7. REVENUE / RENT TRANSACTION

**Entity name:** FactRevenue (target) / Facturación RENTA sheet (current, 3,435 rows)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| InvoiceID (Nº Documento) | Unique invoice number | String | Facturación RENTA | Yes (together with Date/Sociedad) | - | [CONFIRMED] |
| Sociedad | Legal entity | String | Facturación RENTA | Yes (composite key) | FK→DimOrganization | [CONFIRMED] |
| TransactionDate (Fecha Documento) | Invoice/document date | Date | Facturación RENTA | Yes (composite key) | FK→DimTime | [CONFIRMED] |
| PostingDate (Fecha Contable) | Accounting posting date | Date | Facturación RENTA | No | FK→DimTime | [CONFIRMED] |
| BuildingCode (Edificio) | Building code | String | Facturación RENTA | No | FK→DimAsset | [CONFIRMED] |
| Geography | Geographic region | String | Facturación RENTA | No | FK→DimLocation | [CONFIRMED] |
| City | City | String | Facturación RENTA | No | FK→DimLocation | [CONFIRMED] |
| LandlordName (Proveedor) | Landlord/supplier name | String | Facturación RENTA | No | FK→DimLease | [CONFIRMED] |
| LandlordID (CIF) | Landlord tax ID | String | Facturación RENTA | No | - | [CONFIRMED] |
| InvoiceAmount (Importe) | Invoice amount in local currency | Decimal | Facturación RENTA | No | - | [CONFIRMED] |
| AmountInEUR (Importe ML) | Amount in EUR (if multicurrency) | Decimal | Facturación RENTA | No | - | [CONFIRMED] |
| Currency (Moneda) | Currency code | String | Facturación RENTA | No | - | [CONFIRMED] |
| Description (Descripción) | Invoice description/concept | String | Facturación RENTA | No | FK→DimCategory | [CONFIRMED] |
| Concept | Expense concept/category | String | Facturación RENTA | No | FK→DimCategory | [CONFIRMED] |
| PurchaseOrder (Orden-Proyecto) | PO / project reference | String | Facturación RENTA | No | FK→DimProject | [INFERRED] |
| DocumentID (ID doc.) | System document ID | String | Facturación RENTA | No | - | [CONFIRMED] |
| DocumentClass (Clase doc.) | Invoice class (e.g., 01, 02) | String | Facturación RENTA | No | - | [CONFIRMED] |
| InvoiceReference (Referencia) | Vendor reference | String | Facturación RENTA | No | - | [INFERRED] |

[CONFIRMED] 3,435 rent billing transactions from Power Query "Facturación RENTA"; all rows have Sociedad, Date, Amount, Building

---

## 8. AMORTIZATION / FIXED ASSET

**Entity name:** FactAmortization (target) / DATA AMORTIZACIONES sheet (current, 8,240 rows)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| AssetID (Activo) | Unique asset code | String | DATA AMORTIZACIONES | Yes (composite with Sociedad) | - | [CONFIRMED] |
| Sociedad | Legal entity | String | DATA AMORTIZACIONES | Yes (composite) | FK→DimOrganization | [CONFIRMED] |
| AssetClass (Clase) | Asset classification (e.g., RE, IT, MOB) | String | DATA AMORTIZACIONES | No | FK→DimCategory | [CONFIRMED] |
| AssetDescription (Denominación 1/2) | Asset description | String | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| CostCenter | Cost center allocation | String | DATA AMORTIZACIONES | No | FK→DimOrganization | [CONFIRMED] |
| Location (Emplazamiento) | Asset location/building | String | DATA AMORTIZACIONES | No | FK→DimAsset | [CONFIRMED] |
| AcquisitionDate (Fecha Capitalizac) | Asset capitalization date | Date | DATA AMORTIZACIONES | No | FK→DimTime | [CONFIRMED] |
| AmortizationStartDate (Fecha Inicio Amortiz) | Amortization start date | Date | DATA AMORTIZACIONES | No | FK→DimTime | [CONFIRMED] |
| PostingDate (Fecha de contabiliza) | Accounting posting date | Date | DATA AMORTIZACIONES | No | FK→DimTime | [CONFIRMED] |
| AcquisitionValue (Valor adq. act.) | Original acquisition value | Decimal | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| AcquisitionValueEUR (Valor adq.) | Acquisition value in EUR | Decimal | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| UsefulLife (Duración) | Useful life in years | Int | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| AmortizationMethod (Tipo amortización) | Straight-line, accelerated, etc. | String | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| AnnualAmortization (AA ejercicio anterio) | Prior year accumulated amortization | Decimal | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| CurrentYearAmortization (Amortización del últ) | Current year amortization | Decimal | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| AccumulatedAmortization (Amortización acumula) | Total accumulated amortization | Decimal | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| NetBookValue (Valor Neto Contable) | Net book value | Decimal | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| DailyAmortization (Amortizacion Diaria) | Daily amortization amount | Decimal | DATA AMORTIZACIONES | No | - | [CONFIRMED] |
| CalculationPeriod (Período del cálculo) | Period code for amortization | String | DATA AMORTIZACIONES | No | FK→DimTime | [CONFIRMED] |
| JobCode (JOBs) | Internal project/job code | String | DATA AMORTIZACIONES | No | FK→DimProject | [INFERRED] |

[CONFIRMED] 8,240 asset amortization rows from Power Query "DATA - SOLO SÍ"; primary asset classes include RE (Real Estate), IT (Information Technology), MOB (Furniture/Mobility)

---

## 9. CAPEX / INVESTMENT PROJECT

**Entity name:** FactCAPEX (target) / CAPEX + PDI FY-27 sheets (current)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| ProjectID | Unique investment/project code | String | PDI FY-27, CAPEX | Yes (Primary) | - | [INFERRED] |
| ProjectName | Project description | String | PDI FY-27, CAPEX | No | - | [INFERRED] |
| BuildingCode | Asset/building code | String | CAPEX | No | FK→DimAsset | [CONFIRMED] |
| Geography | Geographic region | String | OUTPUT 1 - CAPEX | No | FK→DimLocation | [CONFIRMED] |
| CostCenter | Allocation cost center | String | CAPEX, INPUT | No | FK→DimOrganization | [CONFIRMED] |
| ProjectType | Investment category (RE, IT, MOB, Parking, etc.) | String | CAPEX headers | No | FK→DimCategory | [CONFIRMED] |
| StartDate | Project start date | Date | PDI FY-27 | No | FK→DimTime | [INFERRED] |
| CompletionDate | Project completion/handover date | Date | PDI FY-27 | No | FK→DimTime | [INFERRED] |
| BudgetAmount | Total budget | Decimal | CAPEX, PDI FY-27 | No | - | [CONFIRMED] |
| SpendToDate | Cumulative spend through period | Decimal | CAPEX | No | - | [CONFIRMED] |
| MonthlyAllocation | Monthly cashflow | Decimal | CAPEX (repeats for each month FY26..FY33) | No | FK→DimTime | [CONFIRMED] |
| Scenario | Scenario version (BAU, Optimized, etc.) | String | CAPEX | No | FK→DimScenario | [UNKNOWN] |
| Status | Planned, in-progress, completed, on-hold | String | CAPEX | No | - | [INFERRED] |

[CONFIRMED] CAPEX sheet (1,423 rows × 288 cols) contains detailed monthly investment schedule with multi-year columns (FY26..FY33)

---

## 10. OPEX / OPERATING EXPENSE

**Entity name:** FactOPEX (target) / OPEX sheet (current, 1,302 rows)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| OPEXLineID | Unique OPEX line identifier | String | OPEX | Yes (Primary) | - | [INFERRED] |
| BuildingCode | Asset/building code | String | OPEX, SUPPORT | No | FK→DimAsset | [CONFIRMED] |
| Geography | Geographic region | String | OUTPUT 2 - OPEX | No | FK→DimLocation | [CONFIRMED] |
| CostCenter | Cost center allocation | String | OPEX | No | FK→DimOrganization | [CONFIRMED] |
| ExpenseCategory | OPEX category (Utilities, Parking, GGCC, Taxes, etc.) | String | OPEX | No | FK→DimCategory | [CONFIRMED] |
| Description | Line description | String | OPEX | No | - | [INFERRED] |
| MonthlyAmount | Monthly recurring expense | Decimal | OPEX (repeats for each month FY26..FY33) | No | - | [CONFIRMED] |
| Period | Fiscal period (month/year) | String | OPEX columns | No | FK→DimTime | [CONFIRMED] |
| Source | Source of this line (Billing, Manual, Forecast, etc.) | String | OPEX | No | - | [INFERRED] |
| Scenario | Scenario version (BAU, Optimized, etc.) | String | OPEX | No | FK→DimScenario | [UNKNOWN] |

[CONFIRMED] OPEX sheet (1,302 rows × 269 cols) with 4,225 formulas; includes monthly columns for multi-year budget

---

## 11. TIME / FISCAL PERIOD

**Entity name:** DimTime (target) / Derived from FY columns (current)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| PeriodID | Unique period identifier (YYYYMM or fiscal code) | String | Column headers | Yes (Primary) | - | [INFERRED] |
| FiscalYear | Fiscal year (FY26, FY27, ..., FY33) | Int | CAPEX, OPEX headers | No | - | [CONFIRMED] |
| CalendarYear | Calendar year (2026, 2027, etc.) | Int | Manual | No | - | [INFERRED] |
| Month | Month number (1-12) | Int | Inferred from FY columns | No | - | [INFERRED] |
| MonthName | Month name (January, February, ...) | String | Manual | No | - | [INFERRED] |
| QuarterNumber | Quarter (Q1, Q2, Q3, Q4) | Int | Derived | No | - | [INFERRED] |
| FirstDayOfMonth | First day of month | Date | Derived | No | - | [INFERRED] |
| LastDayOfMonth | Last day of month | Date | Derived | No | - | [INFERRED] |

[CONFIRMED] Multi-year columns FY26 through FY33 detected in CAPEX, OPEX, and reporting sheets

---

## 12. SCENARIO / VERSION

**Entity name:** DimScenario (target) / [UNKNOWN current location]

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| ScenarioID | Unique scenario code | String | [UNKNOWN] | Yes (Primary) | - | [UNKNOWN] |
| ScenarioName | Scenario description (BAU, Optimized, Conservative, etc.) | String | [UNKNOWN] | No | - | [UNKNOWN] |
| Description | Full scenario narrative | String | [UNKNOWN] | No | - | [UNKNOWN] |
| IsBaseline | Whether this is the primary/baseline scenario | Boolean | [UNKNOWN] | No | - | [UNKNOWN] |
| CreatedDate | Date scenario was created/approved | Date | [UNKNOWN] | No | - | [UNKNOWN] |
| ApprovedBy | Approver name/user | String | [UNKNOWN] | No | - | [UNKNOWN] |

[UNKNOWN] Scenario/version management not yet confirmed. Likely in use for what-if analysis but location/structure unknown.

---

## 13. EXPENSE / COST CATEGORY

**Entity name:** DimCategory (target) / INPUT + SUPPORT sheets (current)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| CategoryID | Unique category code | String | INPUT, RESUMEN | Yes (Primary) | - | [INFERRED] |
| CategoryName | Category description | String | INPUT, RESUMEN | No | - | [CONFIRMED] |
| CategoryType | Type (CAPEX, OPEX, Revenue, etc.) | String | RESUMEN | No | - | [CONFIRMED] |
| ParentCategoryID | Higher-level category (NIVEL I → II → III → IV hierarchy) | String | RESUMEN hierarchy | No | FK→DimCategory | [CONFIRMED] |
| Description | Full category narrative | String | INPUT | No | - | [INFERRED] |
| GLAccount | General ledger account code | String | CAPEX, OPEX headers | No | - | [INFERRED] |
| IsActive | Whether category is currently in use | Boolean | Manual | No | - | [INFERRED] |

[CONFIRMED] RESUMEN sheet implements 4-level hierarchical rollups (NIVEL I → II → III → IV) for expense categorization

---

## 14. BUDGET / HEADCOUNT PROJECTION

**Entity name:** FactBudget / FactHeadcount (target) / Dimensionamiento sheet (current)

| Field | Meaning | Data Type | Source | Unique Key? | FK? | Status |
|-------|---------|-----------|--------|-------------|-----|--------|
| ProjectionID | Unique projection/budget ID | String | Dimensionamiento | Yes (Primary) | - | [INFERRED] |
| BuildingCode | Asset/building code | String | Dimensionamiento, SUPPORT | No | FK→DimAsset | [CONFIRMED] |
| FiscalYear | Fiscal year | Int | Dimensionamiento columns | No | FK→DimTime | [CONFIRMED] |
| HeadcountProjected | Projected headcount for period | Int | Dimensionamiento | No | - | [CONFIRMED] |
| SBAProjected | Projected SBA (m²) for period | Decimal | Dimensionamiento | No | - | [CONFIRMED] |
| SUProjected | Projected SU (m²) for period | Decimal | Dimensionamiento | No | - | [CONFIRMED] |
| CostPerM2 | Calculated cost per square meter | Decimal | Dimensionamiento | No | - | [CONFIRMED] |
| CostPerEmployee | Calculated cost per headcount | Decimal | Dimensionamiento | No | - | [CONFIRMED] |
| Scenario | Scenario (BAU, Optimized, etc.) | String | Dimensionamiento | No | FK→DimScenario | [INFERRED] |
| GrowthAssumption | Growth rate applied (e.g., 5% headcount growth) | Decimal | Dimensionamiento (hardcoded cell C3: 0.05) | No | - | [CONFIRMED] |

[CONFIRMED] Dimensionamiento sheet (422 rows × 67 cols) contains headcount and space projections with 346 formulas; hardcoded 5% headcount growth assumption detected in cell C3

---

## Summary of Key Findings

### Confirmed Entities (8):
1. **DimAsset** — Building/workplace (INPUT, SUPPORT) — Multiple sites across Spain
2. **DimPerson** — Employee (Personas, 13,378 rows) — Large headcount master
3. **DimPosition** — Role (Puestos, 396 rows) — Position/job titles
4. **DimOrganization** — SBU/Cost center (Personas, INPUT) — Org hierarchy
5. **DimLocation** — City/Geography (SUPPORT, OUTPUT sheets) — Multi-building regions
6. **FactAmortization** — Fixed asset schedule (DATA AMORTIZACIONES, 8,240 rows, from Query)
7. **FactRevenue** — Rent billing (Facturación RENTA, 3,435 rows, from Query)
8. **FactCAPEX** — Investment projects (CAPEX, 1,423 rows, detailed schedule)

### Likely/Partial Entities (5):
9. **FactOPEX** — Operating expenses (OPEX, 1,302 rows) — Utilities, parking, GGCC, taxes
10. **DimLease** — Contracts (inferred from Facturación RENTA)
11. **DimCategory** — Expense categories (INPUT, RESUMEN hierarchy)
12. **DimTime** — Fiscal/Calendar periods (inferred from FY26..FY33 columns)
13. **FactBudget/FactHeadcount** — Workspace projections (Dimensionamiento, 422 rows)

### Unknown Entities (1):
14. **DimScenario** — Scenario/version management (location/structure unknown)

### Primary Data Quality Issues:
- **BAU FY27 anomaly**: UsedRange extends to row 1,048,463 (very likely corrupted) — [URGENT CLEANUP]
- **External Power Query dependency**: Both revenue and amortization data pulled from SharePoint via Web.Contents() — network/URL risk
- **Large in-workbook tables**: Personas (13k rows), DATA AMORTIZACIONES (8k rows), Facturación RENTA (3.4k rows) — performance and portability risk
- **Hidden sheets and add-in dependencies** (_xlpm.* names): Reproducibility risk if environment unavailable

---

Prepared by: Copilot (Phase 1 Data Dictionary)
Date: 2026-09-01
Based on: Phase 0 evidence (current_model.md, data_sources.md, dependencies.md)
Status: Ready for Phase 1.1 — Logical Data Model Design
