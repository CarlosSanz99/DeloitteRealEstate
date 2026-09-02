# PHASE 1 — DATA MODEL (LOGICAL)

This document defines the target logical data model for the CRE budgeting system based on Phase 0 evidence.

## 1. DIMENSIONS

### DimAsset

One row per unique building/workplace.

| Field | Type | Key | Source | Status |
|-------|------|-----|--------|--------|
| AssetKey | Integer | PK | Generated | [CONFIRMED] |
| AssetID | String | Unique | INPUT, Personas, CAPEX, OPEX | [CONFIRMED] |
| AssetName | String | | INPUT sheet | [CONFIRMED] |
| LocationKey | FK | | DimLocation | [CONFIRMED] |
| CountryCode | String | | INPUT sheet (Geographic hierarchy) | [CONFIRMED] |
| StatusCode | String | | Current / Closed (inferred from closure logic) | [INFERRED] |
| EffectiveDate | Date | | First transaction or audit date | [UNKNOWN] |
| EndDate | Date | | Office closure date (new offices FY26+) | [INFERRED] |

**Grain:** One row per unique Asset across entire historical/projected period.

**Source:** INPUT sheet (master), Personas (employee locations), CAPEX/OPEX (transactions).

**Note:** BAU FY27 sheet contains 422 unique assets in Dimensionamiento layer [CONFIRMED].

---

### DimLocation

One row per unique city/geography.

| Field | Type | Key | Source | Status |
|-------|------|-----|--------|--------|
| LocationKey | Integer | PK | Generated | [CONFIRMED] |
| City | String | Unique | INPUT, Geographic hierarchy (NIVEL I) | [CONFIRMED] |
| Region | String | | Geographic hierarchy | [INFERRED] |
| Country | String | | INPUT | [CONFIRMED] |
| LatLong | String | | [UNKNOWN] |
| SBU | String | FK to DimOrganization | CAPEX/OPEX organizational hierarchy | [CONFIRMED] |

**Grain:** One row per unique city.

**Source:** RESUMEN, CAPEX, OPEX sheets; geographic rollup hierarchy (NIVEL I).

---

## 2. FACTS

### FactRevenue

One row per unique rent transaction/invoice.

**Grain:** One row per Invoice × Sociedad × TransactionDate × Building.

| Field | Type | Key | Source | Status |
|-------|------|-----|--------|--------|
| FactRevKey | Integer | PK | Generated | [CONFIRMED] |
| LeaseKey | FK | | DimLease | [CONFIRMED] |
| AssetKey | FK | | DimAsset | [CONFIRMED] |
| TimeKey | FK | | DimTime | [CONFIRMED] |
| Amount | Number | | Facturación RENTA (monthly rent) | [CONFIRMED] |
| Currency | String | | EUR (assumed) | [INFERRED] |
| TransactionDate | Date | | Facturación RENTA invoice date | [CONFIRMED] |
| InvoiceNumber | String | | Facturación RENTA | [CONFIRMED] |
| IsAccrual | Boolean | | TRUE for accrual; FALSE for paid | [INFERRED] |

**Source:** Facturación RENTA table (Power Query); 3,435 transactions.

---

## 3. RECONCILIATION STRATEGY

The future implementation must prove bit-for-bit reconciliation against Excel outputs:

| Reconciliation Test | Source | Target | Tolerance | Priority |
|---|---|---|---|---|
| Total row count | Excel sheet | Target table | 0 (exact match) | CRITICAL |
| Total revenue by asset | RESUMEN total | FactRevenue.SUM | 0.01 EUR | CRITICAL |
| Total OPEX by asset | RESUMEN total | FactOPEX.SUM | 0.01 EUR | CRITICAL |
| Total CAPEX by project | OUTPUT 1 - CAPEX | FactCAPEX.SUM | 0.01 EUR | CRITICAL |
| Total amortization | DATA AMORTIZACIONES | FactAmortization.SUM | 0.01 EUR | CRITICAL |

---

## FINAL ASSESSMENT

### Main Entities (Confirmed)

1. **DimAsset** — 422 unique buildings/workplaces [CONFIRMED]
2. **DimLocation** — Cities/geographies [CONFIRMED]
3. **DimPerson** — Employees; 13,378 records [CONFIRMED]
4. **DimPosition** — Job titles; 396 records [CONFIRMED]
5. **DimOrganization** — SBU hierarchy [CONFIRMED]
6. **DimLease** — Rental contracts; 3,435 records [CONFIRMED]
7. **DimCategory** — 4-level expense hierarchy [CONFIRMED]
8. **DimTime** — Fiscal periods FY26–FY33 (96 months) [CONFIRMED]

### Facts (Confirmed)

1. **FactRevenue** — Rent/billing transactions; 3,435 rows [CONFIRMED]
2. **FactAmortization** — Fixed asset schedule; 8,240 rows [CONFIRMED]
3. **FactCAPEX** — Capital projects; 1,423 projects [CONFIRMED]
4. **FactOPEX** — Operating expenses; 1,302 line items [CONFIRMED]
5. **FactBudgetHeadcount** — Workplace headcount; 422 assets [CONFIRMED]

### Fact Grains (Confirmed)

- **FactRevenue:** Invoice × Sociedad × Building × Date
- **FactAmortization:** Asset × Month
- **FactCAPEX:** Project × Asset × Category × Month
- **FactOPEX:** Asset × Category × Month
- **FactBudgetHeadcount:** Asset × Position × Month

### Readiness for Phase 2

**Phase 1 Data Model Definition: SUBSTANTIALLY COMPLETE**

**Go/No-Go for Phase 2: CONDITIONAL GO**

**Conditions:**

1. ✓ Data Dictionary complete — 14 entities with field-level definitions [CONFIRMED]
2. ✓ Logical Model complete — Dimensions, facts, grains, relationships defined [CONFIRMED]
3. ✓ Source Mapping complete — Current sheets mapped to target entities [CONFIRMED]
4. ✓ Target Architecture defined [CONFIRMED]
5. ✓ Data Quality Rules documented [CONFIRMED]
6. ✓ Reconciliation Strategy documented [CONFIRMED]
7. ⚠ BAU FY27 Cleanup PENDING — User must delete 1M+ empty rows [BLOCKING]
8. ⚠ DimScenario structure UNKNOWN — Project owner clarification required [REVIEW REQUIRED]
9. ⚠ CAPEX amortization logic UNKNOWN [REVIEW REQUIRED]

**RECOMMENDED NEXT STEP FOR PHASE 2:**

1. **Immediate:** User cleans BAU FY27 sheet (delete empty rows 422–1,048,463).
2. **Then:** Project owner provides clarifications on DimScenario, cross-workbook links, amortization rules.
3. **Then:** Begin Phase 2 — Data Implementation with Python + pandas.
