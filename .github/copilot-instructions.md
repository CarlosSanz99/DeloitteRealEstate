# Copilot instructions — Deloitte Corporate Real Estate

Purpose
- Assist repository contributors to build practical Real Estate + Finance + Data + AI solutions for Deloitte Corporate Real Estate (CRE).

High-level priorities
- Real Estate first: preserve domain logic (NOI, IRR, cap rates, cashflows) and make financial assumptions explicit.
- Business value × reliability × simplicity: prefer deterministic Python/Excel calculations over LLM-only solutions.
- Data quality: validate missing values, units, dates, currencies, duplicates, outliers.
- Deterministic calculations first; use LLMs for interpretation, extraction, and summarization.

Coding & tools
- Prefer Python: pandas, numpy, pathlib, typing, dataclasses, logging, pytest.
- Keep code simple, modular, well-documented, and tested. Add sanity checks for financial outputs.
- Excel artifacts: separate inputs, calculations, outputs; use structured tables; avoid hardcoded values.

AI / LLM usage
- Do not rely on LLMs for deterministic numeric computations. LLMs may be used for lease analysis, document parsing, summaries, and NL queries.
- When designing agents, separate reasoning, tools, data, and deterministic calc layers.

Security and secrets
- Never commit API keys, passwords, or tokens. Use environment variables and .env patterns.

Repository workflow
- Branch naming: kebab-case with a short descriptive name (this session uses `carlossanz99-deloitte-cre-setup`).
- Changes should be surgical and limited in scope; do not rewrite working code without a clear reason.

If unclear
- Ask the repository owner for clarification before making assumptions that affect financial logic or data interpretation.

Contact
- Repository intent: Deloitte Corporate Real Estate analytics and automation. Prioritize commercial usefulness and auditability.
