# Log

Append-only audit trail of **material wiki-changing events**. Newest entries at the bottom.

**Format:** `## [YYYY-MM-DD HH:MM] <verb> | <title>` followed by one or two sentences describing what changed and which pages were touched.

**Verbs:** `ingest`, `lint`, `decision`, `contradiction`, `author`, `query`.

**When entries land here:**

- `ingest` — every `/ingest` run (always logged; summarises pages, decisions, contradictions touched).
- `lint` — every `/lint` run.
- `decision` / `contradiction` — when a query or ingest surfaces one and the user confirms saving it. The entry is in addition to the `/ingest` log entry only for queries; ingests fold these into their own entry.
- `author` — when a `team-inputs/` file is authored or materially revised.
- `query` — rare. Only when the user explicitly asks to record a query. Routine Q&A does **not** log.

---

<!-- Claude appends new entries here. Example shape:

## [2026-04-20 09:15] ingest | Annex 3 — Security/GDPR/AI/BCM requirements

Ingested `client-inputs/2026-04-17-Annex-3-Security-GDPR-AI-BCM-requirements.md`. Created stakeholder hubs (CISO, DPO, Cloud Architect), 8 ADR-seed pages under `technical-architecture/adrs/`, and `contract/clauses/audit-right.md`. Flagged 2 contradictions with the draft DPA template.

-->
