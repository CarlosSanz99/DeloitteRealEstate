# Copilot instructions — Deloitte Corporate Real Estate

Scope and purpose
- This repository is exclusively for the Deloitte Corporate Real Estate (CRE) project. Use this file to guide how GitHub Copilot and contributors should behave when authoring, reviewing, or automating work in this repo.
- High-level objective: progressively transform an existing Excel-based CRE process into a structured, reliable, auditable, and increasingly automated system. Current project phase: PHASE 0 — DISCOVERY AND DOCUMENTATION.

Operational principles (how Copilot must work)
1. Understand before changing. Always inspect and seek to understand existing Excel masters, sheets, formulas, inputs, outputs, mappings and workflows before proposing or implementing changes.
2. Do not invent business rules. Never assume Deloitte-specific rules, financial assumptions, tenant terms, or data schemas. If something is unknown, mark it explicitly as [UNKNOWN] or [TO BE DOCUMENTED].
3. Distinguish facts, assumptions and interpretations. Any assumption must be documented, dated, and attributed.
4. Preserve existing Excel logic until validated. Do not replace or refactor Excel formulas/flows until they have been documented, tested, and approved.
5. Deterministic calculations are authoritative. Financial calculations (NOI, IRR, NPV, cap rates, cashflow waterfalls, loan schedules, etc.) must be implemented deterministically (code or spreadsheet) and validated with unit tests and reconciliation against the Excel master. LLMs are not a source of truth for numeric results.
6. Use LLMs for augmentation only. Primary AI uses: document parsing, lease extraction, text summarization, mapping suggestions, natural-language query interfaces, and decision support — not numeric computation.
7. Data quality is critical. Implement validation checks for missing values, duplicates, inconsistent units, date formats, currency mismatches, mapping errors, outliers and unexpected values. Flag and do not silently drop questionable records.
8. Prioritize business value, reliability, simplicity and auditability when recommending solutions. Prefer minimal, well-tested changes that deliver measurable business benefit.
9. Prefer simple Python-based solutions when automation is warranted. Recommended stack: Python + pandas/numpy + pathlib + typing + dataclasses + logging + pytest. Keep dependencies minimal and well-justified.
10. Avoid unnecessary infrastructure. Do not introduce new databases, external APIs, MCP servers, or heavy frameworks unless there is a clear business case and an approved security plan.
11. Confidentiality and data handling. Treat all Deloitte data as confidential. Never commit credentials, personal data, or confidential datasets. Use anonymized or synthetic data for development whenever possible. Do not upload confidential files to unapproved external services.
12. Lifecycle for substantial changes: inspect → understand → document (docs/) → propose (PR with design and tests) → implement → test → validate → document (update docs/). Follow this sequence without skipping steps.
13. Never claim a feature works without tests. All meaningful behavior must have deterministic tests and example reconciliation cases against the Excel master.
14. When multiple approaches are possible, recommend the simplest reliable approach and explain the business tradeoff briefly.

Phase 0 — Discovery and Documentation (required actions)
- Inventory: enumerate Excel files, sheets, named ranges, key inputs, outputs, and external data sources. Record this inventory in docs/ (not in this file).
- Mapping: for each sheet, capture purpose, inputs, outputs, key formulas, and stakeholders. Use [TO BE DOCUMENTED] for unknown items.
- Data lineage: map source systems and manual processes that populate project data. Mark any automated sources and their refresh cadence.
- Validation plan: define validation checks and reconciliation tests that will prove any reimplementation preserves business outcomes.
- Deliverables for Phase 0: comprehensive docs/ pages describing the Excel master(s), a prioritized list of automation candidates, and a test matrix for validation.

Documentation vs. instructions
- This file defines HOW Copilot should behave in the repo. Detailed business knowledge, Excel specifics, data dictionaries, and process flows belong under docs/ and must be authored during Phase 0.
- Use clear markers in docs/ for items imported from Excel: e.g., "Source: Excel master v1, sheet 'RentRoll'".

Repository hygiene and workflow
- Branches: kebab-case, short descriptive names. Create small PRs with focused scope. Keep changes surgical.
- Commits: clear, descriptive messages. Include test evidence when changing financial logic.
- PRs: lead with why, summarize approach, call out tradeoffs, link to docs/ pages and validation artifacts. If closing an issue, include reference on its own line.

Security and compliance
- Do not commit secrets or PII. Use environment variables and .env patterns for local development only. Add guidance in docs/security.md if needed.
- Use anonymized datasets for public examples. For private CI or runners, ensure secure storage of credentials outside the repo.

Long-term conceptual architecture (do not implement yet)
SOURCE DATA -> DATA INGESTION -> DATA VALIDATION -> REAL ESTATE MASTER -> FINANCIAL ENGINE -> ANALYTICS -> REPORTING -> AI -> AUTOMATION / AGENTS

How to flag unknowns and decisions
- Use [UNKNOWN] for missing facts and [TO BE DOCUMENTED] for items that must be captured in docs/. Record the date and author for assumptions.

Developer guidance
- Prefer readable, well-tested Python over clever one-liners. Add unit tests (pytest) for numeric logic and include sample reconciliations against the Excel master.
- When adding new dependencies, document the business need and limit additions to well-maintained, permissively licensed packages.
- Provide short contextual explanations for non-technical reviewers.

Contact and intent
- Repository intent: Deloitte Corporate Real Estate analytics, automation and modernization, with an emphasis on auditability and business correctness.
- If in doubt, ask the repository owner or stakeholders before making changes that affect financial logic or data handling.

Phase: PHASE 0 — DISCOVERY AND DOCUMENTATION

[END]
