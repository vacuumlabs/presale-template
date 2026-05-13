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

## 2026-05-13 — Diagram tool: Mermaid for wiki, draw.io for complex architecture

- **Decision:** Inline/summary diagrams in wiki pages use Mermaid (renders in GitHub, no binary file needed). Complex architecture diagrams where visual layout matters use draw.io via the draw.io MCP; these require a `.md` companion file alongside the `.drawio` binary. P3b (capabilities map) and P4b (architecture) use draw.io; any other diagram defaults to Mermaid.
- **Context:** Using Mermaid for everything avoids the binary companion rule overhead on simple diagrams and makes wiki pages readable in GitHub. draw.io remains available for cases where diagram layout is architecturally significant.
- **Made by:** Boris Vida (Step 1.5 content review)
- **Status:** active

## 2026-05-13 — P8 proposal review finding severity definitions

- **Decision:** Three severity levels for P8 quality check findings: **Critical** = blocks sending (missing scope section, broken or missing estimate, no key assumptions stated); **Important** = must be fixed before sending (weak client narrative, uncovered material risk, wrong section numbers, missing UAT/hypercare phases); **Minor** = address if time permits (phrasing improvements, completeness of supporting sections, style).
- **Context:** Severity levels were undefined in V1, making it hard to prioritise P8 findings under time pressure.
- **Made by:** Boris Vida (Step 1.5 content review)
- **Status:** active

## 2026-05-13 — Add Business Architecture questions to T2 (Section 6)

- **Decision:** T2 gains a new Section 6 ("Operating Model & Success", 4 questions: affected teams, new internal capabilities needed, year-after-go-live picture, success KPIs). The old "Open Questions" section renumbered to Section 7. Section 6 is marked optional for T1 engagements.
- **Context:** P3e (Business Architecture) needs raw material from discovery on how teams' work changes and how the client will measure success. This was not captured in T2 before.
- **Made by:** Boris Vida (Step 1.5 content review)
- **Status:** active

## 2026-05-13 — Clarify AI productivity in the single-estimate model (SA accountability)

- **Decision:** The 2026-05-13 decision to drop the AI-assisted vs baseline comparison removed the *client-facing* split, not AI productivity itself. AI-tooled productivity remains baked into the single estimate. The SA is responsible and accountable for stating these assumptions explicitly in `estimation-dev.md` under a dedicated `## AI productivity (internal)` subsection and confirming them at ARB. P5 drafts them; the SA double-checks every line before accepting the estimate.
- **Context:** Risk that "drop AI/baseline split" was read as "ignore AI productivity entirely", which would produce defensively-conservative estimates that lose deals on price. AI productivity is real and assumed; it just isn't surfaced as a marketing line to the client.
- **Made by:** Boris Vida
- **Refines:** "Drop AI-assisted vs baseline comparison from estimates" (2026-05-13). Both decisions are active together.
- **Status:** active
