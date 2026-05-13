---
description: Run problem framing — problem statement, capabilities map, in-scope flows, known unknowns
argument-hint: (no arguments needed)
---

# P3 — Problem Framing

Produce four outputs that give a complete picture of the client's business problem before technical work begins: (1) a problem statement in client language, (2) a business capabilities map, (3) the key flows to build or change, and (4) a structured list of what we still don't know.

**Run sequentially.** Each output must be reviewed before running the next part. This is not a batch operation.

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `team-inputs/T2-discovery-qa.md` — filled in during/after the discovery call
- `product-management/domain-research.md` — from P2
- `product-management/regulatory-brief.md` — from P2
- Budget signal in T1 brief is confirmed or escalated

If T2 is missing or empty, stop: "P3 needs discovery call notes. Fill in `team-inputs/T2-discovery-qa.md` and run `/ingest team-inputs/T2-discovery-qa.md`, then run `/p3`."

If budget signal is still Unknown, note it prominently: this does not block P3, but P4 cannot begin without resolving it.

## 1. P3a — Problem statement

**Run first. Review before continuing to P3b.**

Using the T2 discovery Q&A, domain research, and any client-inputs transcripts or notes in the wiki, produce a structured problem statement.

Structure:

**What they said** — direct quotes or close paraphrases of the problem as the client articulated it. Use their words, not VL's.

**What they need** — your interpretation of the underlying business problem, stripped of implementation assumptions.

**What success looks like for them** — specific, measurable outcomes in their terms. Not "a modern platform" — concrete operational changes.

**Key tensions** — the competing constraints or priorities the client is navigating.

Mark each data point:
- **[CLIENT]** — confirmed by the client in workshop or questionnaire
- **[PUBLIC]** — inferred from public sources
- **[VL]** — VL analysis or assumption

Create `product-management/problem-statement.md`:

```yaml
---
type: concept
title: Problem statement — <client name>
tags: [problem, discovery, P3]
sources:
  - ../team-inputs/T2-discovery-qa.md
  - ../product-management/domain-research.md
last_updated: <today>
status: draft
---
```

**Stop here. Ask the user to review `problem-statement.md` before continuing.**

## 2. P3b — Business capabilities map

**After problem-statement.md is reviewed and accepted.**

Using the problem statement and discovery notes, produce a business capabilities map for the client.

Using the draw.io MCP server, generate a capabilities diagram showing:
- **Business capability domains** — group related capabilities into logical domains
- **Capabilities within each domain** — what the business does (not systems)
- **Key actors** — end users, internal staff, third-party entities

Mark capabilities that are currently broken, limited, or cited as pain points using a distinct colour.

Save the `.drawio` file to `technical-architecture/` (capabilities maps live here as precursors to architecture). Create a mandatory companion `.md` file next to it summarising the key capability domains, actors, and pain areas.

Create `product-management/capabilities-map.md` — a markdown summary with the diagram embedded/linked:

```yaml
---
type: concept
title: Capabilities map — <client name>
tags: [capabilities, architecture, P3]
sources:
  - ../team-inputs/T2-discovery-qa.md
  - product-management/problem-statement.md
last_updated: <today>
status: draft
---
```

Body: text summary of each capability domain, which actors interact with it, and what is in pain. Embed a Mermaid block or link to the `.drawio` file.

**Stop here. Ask the user to review the capabilities map before continuing.**

## 3. P3c — In-scope flows

**After capabilities-map.md is reviewed and accepted.**

Using the problem statement and capabilities map, identify the flows in scope for this engagement. Review with the SA before writing this page.

For each flow:
- **Flow name** — short, descriptive
- **Actors involved** — who initiates, who approves, who is affected
- **Happy path** — the expected sequence when everything works
- **Key edge cases** — the most important variations or exceptions
- **Current pain** — what is broken, slow, or manual in the current version
- **Target state** — what this flow looks like after the engagement
- **Open questions** — anything unknown that would affect how it is built

Mark each data point [CLIENT], [PUBLIC], or [VL].

Create `product-management/in-scope-flows.md`:

```yaml
---
type: concept
title: In-scope flows — <client name>
tags: [scope, flows, P3]
sources:
  - ../team-inputs/T2-discovery-qa.md
  - product-management/problem-statement.md
  - product-management/capabilities-map.md
last_updated: <today>
status: draft
---
```

**Stop here. Ask the user to have the SA review in-scope-flows.md before continuing.**

## 4. P3d — Known unknowns

**After in-scope-flows.md is SA-reviewed and accepted.**

Using all P3 outputs, produce a structured list of what is not yet known about this engagement. Group by risk:

**Blockers** — must be resolved before architecture can begin (budget confirmation, integration scope, platform choice).

**Important — resolve before proposal** — should be clarified in the next client call or async (data migration approach, regulatory deadlines, existing team capabilities).

**Can wait until Discovery** — can be discovered during paid Phase 0 (exact data model, third-party API specifics, internal approval workflows).

For each item: what we don't know, why it matters (what decision does it affect?), and the clarifying question to ask the client.

Create `product-management/known-unknowns.md`:

```yaml
---
type: concept
title: Known unknowns — <client name>
tags: [unknowns, risks, P3]
sources:
  - product-management/problem-statement.md
  - product-management/in-scope-flows.md
last_updated: <today>
status: draft
---
```

**Any Blockers must be resolved or escalated before the G2 gate.**

## 5. What comes next

After P3d is complete, two more steps are required before G2 for T2/T3 deals:
- Run `/p3e` — Business Architecture (stakeholders, value streams, operating model impact, success measures)
- Run `/p3f prepare` — prepare agenda for Sales Strategy Alignment call (T7)

Then run `/p3f document` after the alignment call to complete T7 before passing G2.

## 6. Update indexes

For each new page created:
- Add an entry under the correct heading in root [`index.md`](../../index.md)
- Add an entry to `product-management/README.md` under its `## Index` section

## 7. Append to log.md

Append after each part completes:

```
## [YYYY-MM-DD HH:MM] ingest | P3 problem framing — <client name>

P3 complete for <client name>. Four pages created in product-management/.
- Created: problem-statement.md — client-language problem statement (P3a)
- Created: capabilities-map.md — business capability domains and pain areas (P3b)
- Created: in-scope-flows.md — <N> flows in scope with actors and target states (P3c)
- Created: known-unknowns.md — <N blockers / N important / N deferred> unknowns (P3d)
```

## 8. Report back

After all four parts complete, tell the user:
- All four pages created
- Number of Blockers in known-unknowns.md (these need resolution before G2)
- Whether budget signal has been resolved
- What to do next: run `/p3e` and then `/p3f prepare` before the G2 gate (T2/T3)

Changes are committed and pushed automatically when this turn ends.
