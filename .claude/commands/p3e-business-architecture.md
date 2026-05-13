---
description: Produce the Business Architecture — stakeholder RACI, value streams, operating model impact, success measures
argument-hint: (no arguments needed)
---

# P3e — Business Architecture

Produce the business architecture for the engagement. This covers the organisational and operational dimension of the client's problem: who is affected, how they are affected, and how success will be measured.

This feeds two downstream consumers: the G2 gate review (required for T2/T3), and T5 §5 (Business Architecture section of the proposal).

## 0. Check required inputs

Check that the following exist (read `index.md` to verify):
- `product-management/problem-statement.md` (P3a)
- `product-management/capabilities-map.md` (P3b)
- `product-management/in-scope-flows.md` (P3c)
- `product-management/domain-research.md` (P2a)

If any are missing, stop and list what must be created first.

Also read T2 (`team-inputs/T2-discovery-qa.md`) Section 6 ("Operating Model & Success") — questions 22–25 are the primary raw material for this analysis.

## 1. Stakeholders and actors

Produce a complete stakeholder picture for the engagement.

**Stakeholder table:**

| Name / Role | Group | Primary interest in this engagement | Authority |
|-------------|-------|-----------------------------------|-----------|
| | | | Sponsor / Approver / Consulted / Informed |

Cover: client-side executive sponsor(s), project owner, business owners of each in-scope domain, compliance/legal stakeholder if regulatory work is in scope, and end users as a group.

**Actor list:**

Human actors: end users (customers), internal operations staff, relationship managers, admin/support, compliance officers — whoever actually uses the system being built.

System actors: existing core banking system, third-party services (KYC, payment schemes, etc.), any external systems in the integration map.

**Diagram:**

Produce a simple context diagram using Mermaid showing the key actors and their relationship to the system being built. Keep it to 1 page.

## 2. Value streams

Identify 3–5 end-to-end value streams that scope this engagement.

A value stream is a sequence from a trigger to a delivered outcome. Examples: "Onboard a retail customer", "Open a savings account", "Process a card transaction dispute".

For each value stream:
- Name
- Stakeholder receiving value
- Stages (3–6 steps)
- Capabilities used per stage (cross-reference `capabilities-map.md`)
- Value delivered

Present as a summary table (one row per stream) and a half-page flow description for the most important one.

Explicitly cross-reference §3 in-scope flows — every in-scope flow should map to at least one value stream.

## 3. Responsibility split (RACI)

Produce a responsibility matrix at workstream or capability cluster level — not task level.

Parties: Bank / Client, Vacuumlabs, Platform Vendor (if applicable, e.g. Thought Machine), other SIs or third parties.

Coverage:

| Workstream | Client | VL | Platform Vendor | Other |
|-----------|--------|-----|----------------|-------|
| Solution design | | | | |
| Platform configuration | | | | |
| Integration build | | | | |
| Data migration | | | | |
| Testing (SIT) | | | | |
| UAT coordination | | | | |
| Regulatory sign-off | | | | |
| Production cutover | | | | |
| Hypercare | | | | |
| Post-go-live BAU | | | | |

Use R (Responsible), A (Accountable), C (Consulted), I (Informed).

Flag any responsibility ambiguities — areas where it is unclear whether VL or the client owns something. These are scope risks and must be surfaced.

## 4. Operating model impact

How does the client's organisation change after this engagement?

**Teams affected:** which client teams will work differently? What changes in their day-to-day?

**New capabilities or roles needed:** what must the client build internally to run the new system? Examples: a product configuration team for a TM Vault implementation; a new digital operations process.

**Training and change management:** headline-level only. What change readiness is needed? Does the client have a plan?

**Link to client-side commitments:** note what these implications mean for §8.3 of the proposal (Client-Side Commitments).

## 5. Outcomes and success measures

Map the engagement's outcomes to measurable metrics.

Format: capability change → operational metric → business outcome.

Example: "Product configuration in TM Vault → time-to-launch from 6 months to 2 weeks → ability to compete with neobanks on speed."

3–5 outcomes. Each must be:
- Tied to a specific in-scope capability
- Expressed as a measurable change in an operational metric
- Connected to a business outcome that the client cares about (from `problem-statement.md` §2.4)

These become the success measures the proposal commits to.

## 6. Create the wiki page

Create `product-management/business-architecture.md`:

```yaml
---
type: concept
title: Business architecture — <client name>
tags: [business-architecture, RACI, value-streams, P3e]
sources:
  - ../team-inputs/T2-discovery-qa.md
  - product-management/problem-statement.md
  - product-management/capabilities-map.md
  - product-management/in-scope-flows.md
last_updated: <today>
status: draft
---
```

Body: all five sections above.

## 7. Update indexes

- Add entry in root [`index.md`](../../index.md)
- Add entry in `product-management/README.md` under `## Index`

## 8. Append to log.md

```
## [YYYY-MM-DD HH:MM] ingest | P3e business architecture — <client name>

Business architecture created for <client name>.
- Created: product-management/business-architecture.md
- Stakeholders: <N> identified
- Value streams: <N> mapped to in-scope flows
- RACI: <N> ambiguities flagged
- Success measures: <N> outcome chains defined
```

## 9. Report back

Tell the user:
- Page created
- Any RACI ambiguities flagged (these are scope risks)
- The most important success measure (the one the client cares about most)
- What to do next: "Run `/p3f prepare` to generate the agenda for the Sales Strategy Alignment call (T7). This is required before G2 for T2/T3 deals."

Changes are committed and pushed automatically when this turn ends.
