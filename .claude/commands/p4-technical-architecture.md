---
description: Run technical architecture — VL asset scan, architectural approach, components, tech stack, diagrams
argument-hint: (no arguments needed)
---

# P4 — Technical Architecture & Solutioning

Produce the technical architecture: VL asset and reuse scan, architectural approach, components, tech stack, infrastructure, integration patterns, and key risks.

This is the most iterative stage — expect multiple sessions, not a single output. P4a (asset scan) runs first; P4b runs in two sequential parts.

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `product-management/problem-statement.md` (P3a)
- `product-management/capabilities-map.md` (P3b)
- `product-management/in-scope-flows.md` (P3c)
- `product-management/known-unknowns.md` (P3d)
- `product-management/business-architecture.md` (P3e)
- `deal-context/sales-strategy-alignment.md` (P3f / T7) — **required for T2/T3**
- `product-management/domain-research.md` and `product-management/regulatory-brief.md` (P2)
- Budget signal: read from T1 brief

If any of P3a–P3e is missing, stop: "P4 requires all P3 outputs. Run `/p3` and `/p3e` first."

If `deal-context/sales-strategy-alignment.md` is missing for a T2/T3 deal, stop: "P4 requires the Sales Strategy Alignment brief (T7). Run `/p3f prepare` to prepare the alignment call agenda, hold the call, then run `/p3f document` to fill in T7. Then re-run `/p4`."

If budget signal is Unknown, note this prominently and proceed with a three-option architecture (Lean / Standard / Full). If it is Hard or Soft, note the budget constraint as a design input.

## 1. P4a — VL asset scan

**Run before any architecture work.** The output informs every architectural decision.

Search the GDrive Deals folder (ID: `160UcArBo0aG6eLHWwVkN8ZVrWLl9sdbv`) using the Google Drive MCP for relevant VL reusable assets and past deal patterns. Search terms: the engagement domain, the platform if named in T1, similar client types.

**If GDrive MCP is unavailable**, create the page with a placeholder and note clearly what needs to be filled in manually.

Produce a project-specific brief covering:

**Applicable VL assets** — for each relevant asset or accelerator: name and description, relevance to this engagement, fit assessment (as-is or requires customisation), estimated effort saving.

**Relevant past deals** — for each similar past engagement: what made it similar, key patterns to reuse, risks to watch.

**Recommended starting points** — what to treat as given (reuse as-is), what to adapt, and what needs to be built from scratch.

Create `technical-architecture/vl-assets.md`:

```yaml
---
type: concept
title: VL assets and reuse — <client name>
tags: [assets, reuse, P4]
sources:
  - product-management/in-scope-flows.md
  - product-management/capabilities-map.md
last_updated: <today>
status: draft
---
```

**Stop here. Ask the user to review vl-assets.md before continuing to P4b-i.**

## 2. P4b-i — Architectural approach, components & integration map

**After vl-assets.md is reviewed and accepted.**

Budget signal is: [read from T1 brief — HARD €X / SOFT €X–Y / UNKNOWN → three options needed]

Produce Part 1 of the architecture document:

**Architectural approach** — one paragraph summarising the core architectural idea and the reasoning behind it. If platform selection is open (e.g. Thought Machine Vault vs. Mambu), resolve it here first with a clear rationale.

**Components** — every significant system component. For each: name and responsibility, build vs. buy vs. configure, key decisions or assumptions.

**Integration map** — every external system the solution must connect to. For each: direction, data exchanged, key constraints, risk.

If budget signal is Unknown, produce three variants (Lean / Standard / Full) for the architectural approach and component list, with effort and scope trade-offs clearly stated for each.

Create `technical-architecture/architecture.md` (Part 1 only at this stage):

```yaml
---
type: adr
title: Architecture — <client name>
tags: [architecture, technical, P4]
sources:
  - technical-architecture/vl-assets.md
  - product-management/in-scope-flows.md
  - product-management/known-unknowns.md
  - deal-context/sales-strategy-alignment.md
last_updated: <today>
status: draft
---
```

**Stop here. Ask the user to settle the approach, component list, and integration map before continuing to P4b-ii.**

## 3. P4b-ii — Tech stack, infrastructure, integration patterns, reuse & risks

**After P4b-i is settled.**

Extend `technical-architecture/architecture.md` with the remaining sections:

**Tech stack** — for each layer (backend, frontend, data, infrastructure): chosen technology and a one-line rationale.

**Infrastructure** — cloud provider, hosting model, environments, key infrastructure components. Note data residency or regulatory constraints that shaped these choices.

**Integration patterns** — for each integration from P4b-i: pattern (sync/async/batch), rationale, implementation notes.

**VL reuse and accelerators** — draw from `vl-assets.md`. For each reuse opportunity: which component uses it, as-is or adapted, remaining build effort. Note these are commercial commitments — be accurate.

**Key risks and open questions** — top 5 architectural risks with likelihood (H/M/L), impact (H/M/L), and mitigation.

Update `technical-architecture/architecture.md` with these sections.

## 4. Architecture diagrams

After P4b-ii is settled:

Using the draw.io MCP server, generate:
1. A C4 Level 1 (System Context) diagram
2. A C4 Level 2 (Container) diagram

Save the `.drawio` binary to `technical-architecture/`. Create a mandatory companion `.md` file next to it with a short description of each diagram and what it shows.

**Expect 10–20 minutes of manual cleanup.** Note any draw.io limitations and what the SA needs to adjust manually.

Also update `technical-architecture/architecture.md` to reference the diagram files with relative links.

## 5. Update indexes

For each new or updated page:
- Add/update entry in root [`index.md`](../../index.md)
- Add/update entry in `technical-architecture/README.md` under its `## Index` section

## 6. Append to log.md

```
## [YYYY-MM-DD HH:MM] ingest | P4 technical architecture — <client name>

P4 complete for <client name>. Architecture pages created in technical-architecture/.
- Created: vl-assets.md — <N> relevant VL assets identified
- Created/updated: architecture.md — approach, <N> components, <N> integrations, tech stack, infra, risks
- Created: <diagram-filename>.drawio + .md — C4 L1 and L2 diagrams
```

## 7. Report back

Tell the user:
- All pages created or updated
- Whether budget signal required three-option architecture
- The top architectural risk flagged in §8 (Risks)
- Any reuse claims in §7 that need SA confirmation before the proposal (they are commercial commitments)
- What to do next: "Run `/p5` after architecture is settled. For T2/T3, also confirm that G3 (ARB) is scheduled."

Changes are committed and pushed automatically when this turn ends.
