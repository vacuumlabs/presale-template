# Decisions

Every meaningful call made across this engagement. Each entry links to its supporting ADR, clause, change request, or meeting note so the *why* is recoverable years from now.

**Format:**

```
## YYYY-MM-DD — <short title>

- **Decision:** what was decided, in one sentence
- **Context:** why this was on the table
- **Made by:** person(s) / role(s)
- **Sources:**
  - [link to supporting ADR / clause / note]
- **Supersedes:** <optional — prior decision this replaces>
- **Status:** active | reversed | superseded
```

Claude appends entries here whenever a decision surfaces during `/ingest` or while answering a project question. If you make a decision in a conversation without Claude, log it here yourself or ask Claude to do it.

---

<!-- Newest at the bottom. Claude maintains this list. -->

## 2026-05-13 — Drop AI-assisted vs baseline comparison from estimates

- **Decision:** T5 §9 and P5 output use a single estimate (no baseline/AI-assisted split, no mention of AI tooling). The number presented to clients is the estimate — full stop.
- **Context:** The comparison added noise and raised questions about AI reliance. The estimate already reflects VL's standard way of working; surfacing it as "AI-assisted" is unnecessary and potentially distracting.
- **Made by:** Boris Vida (Step 1.5 content review)
- **Status:** active

## 2026-05-13 — Restructure estimation process (P5)

- **Decision:** Bottom-up estimation applies to development work only. Overhead roles (PM, product management, QA, design, infra) are estimated as separate fixed allocations (fractional FTEs per phase, not percentages of dev). Cross-check between feature-based and team×timeline methods applies to development only, then overhead is added on top. Output files: `estimation-dev.md`, `estimation-overhead.md`, `estimation-final.md`.
- **Context:** Old V1 process (P5a–P5f with Method A/B across all work) was too complex and conflated dev effort with overhead. New structure is cleaner and more defensible.
- **Made by:** Boris Vida (Step 1.5 content review)
- **Status:** active

## 2026-05-13 — Add UAT, go-live, hypercare, handover as required proposal phases

- **Decision:** T5 §7.1 phases table must always include UAT, go-live & release, hypercare (2–4 weeks), and handover. These are not optional — every T2/T3 proposal includes them. T5 §9.2 estimate by phase also includes them as line items.
- **Context:** These phases were previously absent from the template, leading to proposals that looked cheaper or shorter than reality.
- **Made by:** Boris Vida (Step 1.5 content review)
- **Status:** active
