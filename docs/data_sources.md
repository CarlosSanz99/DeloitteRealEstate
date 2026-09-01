# Power Query Analysis — PRESUPUESTO FY27 - ANALISIS.xlsx (Phase 0 discovery)

## Overview
Two Power Query queries were detected and extracted read-only. Both pull external Excel files from SharePoint and apply type transformations. Below are the full documented details.

## 1. Query: "DATA - SOLO SÍ"

**Source (confirmed):** External Excel file via Web.Contents()
- URL: `https://resources.deloitte.com/sites/CORPORATEREALESTATE/Shared%20Documents/CONTROL%20ECONOMICO/01.%20Financiero/01.%20Contabilidad/Amortizaciones/Resumen%20Formulado/00_Resumen%20Amortizaciones.xlsx`
- Sheet within source: "DATA - SOLO SÍ"

**Transformations (confirmed):**
1. Extract sheet named "DATA - SOLO SÍ" from external workbook
2. Promote first row to headers (PromoteHeaders)
3. Apply type coercion to all columns:
   - Int64: Sociedad, Activo, Determ.cuentas, Clase, Centro de Coste, Emplazamiento, Numero inventario, Nº serie, Duración, Período del cálculo, cuenta, Días, Duracion
   - Date: Fecha Inicio Amortiz, Fecha de contabiliza, Fecha de capitalizac, Fecha de Fin
   - Text: Descrip.emplazamient, Descrip.CeCo, Denominación 1, Denominación 2, JOBs, Tipo amortización, CONTAR AMORT CRE2
   - Number: Valor adq. act., Valor adq., AA ejercicio anterio, Amortización acumula, Amortización del últ, AA Total, Valor Neto Contable, Amortizacion Diaria

**Output destination (confirmed):**
- Loaded into table "DATA___SOLO_SÍ" on worksheet "DATA AMORTIZACIONES"
- Also refreshes external connection "Query - DATA - SOLO SÍ"

**Purpose (inferred):** Fixed asset amortization schedule and historical accounting data from central CRE amortizations file. [CONFIRMED]

---

## 2. Query: "Facturación RENTA"

**Source (confirmed):** External Excel file via Web.Contents()
- URL: `https://resources.deloitte.com/sites/CORPORATEREALESTATE/Shared%20Documents/PORFOLIO/00.%20GENERAL/01.%20PORTFOLIO%20DATA/Deloitte%20Data.xlsx`
- Sheet within source: "Facturación RENTA"

**Transformations (confirmed):**
1. Extract sheet named "Facturación RENTA" from external workbook
2. Promote first row to headers
3. Apply type coercion to all columns:
   - Int64: Sociedad, Nº Documento, Multisociedad, Doc.compras
   - Text: CIF, Proveedor, Clase doc., Cliente, Orden-Proyecto, Ind. Impuesto, Descripción, ID doc., Referencia, Ext. Ref. SII, Diferencias, Extrae Igual, Concepto, Edificio, Geografía, Ciudad, Left 20
   - Number: Importe, Importe ML
   - Date: Fecha Contable, Fecha Documento
   - (Moneda as text)

**Output destination (confirmed):**
- Loaded into table "Facturación_RENTA" on worksheet "Facturación RENTA"
- Also refreshes external connection "Query - Facturación RENTA"

**Purpose (inferred):** Rent billing and invoicing transactions at monthly/transactional grain, by building/geography/city. Data feeds rent revenue and OPEX calculations. [CONFIRMED]

---

## Dependencies Between Queries

No direct dependencies detected between the two queries. Each pulls from an independent external source.

## Data Refresh & Refresh Paths

Both queries use `Web.Contents()` with external URLs, indicating:
- Web-based refresh (requires internet connectivity to resources.deloitte.com)
- Refresh triggered on workbook open or manual refresh (unless scheduled via Power Automate)
- Cached data within the workbook between refreshes

**Risk:** Broken URLs or connectivity issues will break both queries. URLs appear stable (SharePoint paths).

---

Prepared by: Copilot (Power Query M formula extraction, read-only)
Date: 2026-09-01

