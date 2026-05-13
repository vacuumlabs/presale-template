---
description: Run domain research — client profile, regulatory brief, competitive positioning
argument-hint: (no arguments needed)
---

# P2 — Domain Research

Three research passes to run before the first client call. Together they give the presale team enough context to arrive informed. Run all three in one session; each output feeds the next.

Follow all steps in order.

## 0. Check required inputs

Check that the following exist in the wiki (read `index.md` to verify):
- `deal-context/` contains intake outputs from P1 (pre-meeting brief, intake gaps)
- `team-inputs/T1-sales-deal-brief.md` exists and is filled in

If either is missing, stop and tell the user exactly what to run first.

## 1. P2a — Client & domain research

Using the T1 brief and P1 outputs as context, plus publicly available information, produce a research brief on the client and their vertical.

Cover:

**Client profile** — company overview, market position, size/stage, products and services they offer (as publicly described), recent news or strategic moves relevant to this engagement, visible pain points.

**Domain context** — how this vertical works in the client's market and region, typical business model and revenue drivers, key market dynamics (growing, consolidating, being disrupted), where technology is currently a constraint or enabler.

**Likely priorities** — based on client profile and vertical, the top 3 things a company like this is most likely trying to achieve right now.

Mark clearly what is confirmed from public sources vs. inferred.

Create `product-management/domain-research.md`:

```yaml
---
type: concept
title: Domain research — <client name>
tags: [domain, research, P2]
sources:
  - ../team-inputs/T1-sales-deal-brief.md
last_updated: <today>
status: draft
---
```

## 2. P2b — Regulatory & compliance brief

Using the client and domain context now in `product-management/domain-research.md`, produce a regulatory brief.

Cover:

**Key regulatory framework** — which regulatory bodies govern this vertical in this market; primary regulations or directives that apply (e.g. central bank licensing, PSD2/open banking, AML/KYC obligations, GDPR/data residency).

**Constraints most likely to affect this engagement** — data residency, licensing requirements, audit and reporting obligations that would affect system design, upcoming regulatory changes the client may need to comply with.

**VL's relevant experience** — has VL dealt with this regulatory environment before? What did we learn?

Flag the 2–3 regulatory factors most likely to constrain architecture or timeline.

Create `product-management/regulatory-brief.md`:

```yaml
---
type: concept
title: Regulatory brief — <client name>
tags: [regulatory, compliance, P2]
sources:
  - ../team-inputs/T1-sales-deal-brief.md
  - product-management/domain-research.md
last_updated: <today>
status: draft
---
```

## 3. P2c — Competitive positioning

Using the T1 brief, domain research, and regulatory brief, produce a competitive positioning brief.

Cover:

**Likely competitors** — who else is likely to be evaluated for this type of engagement: system integrators with fintech practices, specialist boutiques, platform vendors who bundle implementation, local technology companies. For each: their likely pitch, strengths, and weaknesses relative to VL.

**VL's differentiated position** — what VL can credibly claim that likely competitors cannot easily match: specific domain expertise (Thought Machine Vault depth, AI-native fintech), delivery model, track record.

**Risks to VL's position** — where is VL most vulnerable in this competitive context?

Be honest. A realistic competitive picture is more useful than an optimistic one.

Create `product-management/competitive-positioning.md`:

```yaml
---
type: concept
title: Competitive positioning — <client name>
tags: [competitive, positioning, P2]
sources:
  - ../team-inputs/T1-sales-deal-brief.md
  - product-management/domain-research.md
  - product-management/regulatory-brief.md
last_updated: <today>
status: draft
---
```

## 4. Update indexes

Follow the two-level index rule. For each new page:
- Add an entry under the correct heading in root [`index.md`](../../index.md)
- Add an entry to `product-management/README.md` under its `## Index` section

## 5. Append to log.md

Append to [`log.md`](../../log.md):

```
## [YYYY-MM-DD HH:MM] ingest | P2 domain research — <client name>

P2 ran for <client name>. Three pages created in product-management/.
- Created: domain-research.md — client profile, domain context, likely priorities
- Created: regulatory-brief.md — key regulatory constraints for this market/vertical
- Created: competitive-positioning.md — likely competitors and VL's differentiated position
```

## 6. Report back

Tell the user:
- Which pages were created
- The 2–3 most important regulatory constraints to watch for this engagement
- VL's strongest competitive differentiator for this client
- What to do next: "Review these three pages before the discovery call. Run `/p3` after the first client workshop or discovery call."

Changes are committed and pushed automatically when this turn ends.
