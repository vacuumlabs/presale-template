# T5 — Proposal Document Template

This is the standard structure for a Vacuumlabs T2 proposal document. Each section notes which SDLC deliverable it draws from, so the PM knows exactly where to pull content.

**Owner:** PM assembles; SA owns technical sections; Sales owns commercial terms and final narrative tone.
**Format:** Google Doc. This template defines structure and content expectations — not visual design.
**T1 deviation:** Sections 3–6 compressed into a 1-pager. See note at the bottom.
**T3 deviation:** Add compliance matrix and formal RFP response structure after Section 8.

> **Not all sections are required for every deal.** For T1 (ballpark) or other lightweight engagements, fill in only what is necessary. The minimum viable proposal contains: §1 Executive Summary, §3 Scope, §9 Estimate (summary table + key assumptions), and §12 Next Steps. Everything else — architecture diagrams, full risk table, §8 Team, §11 Why Vacuumlabs — is optional and should only be included if it adds value for this specific client and engagement type.

_Run `/p7` to generate first drafts of all sections from accumulated project context._

---

## Cover Page

* Client name and logo
* Proposal title (e.g. "Core Banking Transformation — Proposal")
* Prepared by: Vacuumlabs
* Date
* Version / status (Draft / Final)
* Confidentiality notice

---

## 1. Executive Summary

**Length:** 1 page maximum.
**Source:** Synthesised from all SDLC outputs. Written last, after all other sections are complete.
**Owner:** PM drafts; Sales refines tone.

Content:

* What we understood about the client's challenge (2–3 sentences in client language)
* What we are proposing (approach in plain terms — no jargon)
* Headline delivery plan: phases, total timeline, first value delivery date
* Headline estimate: recommended range (baseline and AI-assisted if applicable)
* Why Vacuumlabs is the right partner for this (1–2 sentences)
* Proposed next step

> The Executive Summary is the only section most senior stakeholders will read in full. It must stand alone.

---

## 2. Understanding Your Challenge

**Source:** `problem-statement.md` (P3a)
**Owner:** PO drafts; SA reviews for technical accuracy.

### 2.1 Your Situation

What the client is trying to achieve and the business context they are operating in. Written in their language, not VL's. Reference specific things they said in discovery — do not use generic fintech framing.

### 2.2 The Core Challenge

What they actually need, beneath the surface request. The real business problem and what changes in their business if this engagement succeeds.

### 2.3 Key Tensions

The competing constraints the client is navigating (compliance vs. speed, rebuild vs. budget, quick wins vs. platform stability). Naming these shows we understood the real difficulty.

### 2.4 What Success Looks Like

In their terms. Specific, measurable outcomes — not "a modern platform" but "able to launch a new product in 2 weeks" or "operations team recovers 3 days per month."

---

## 3. Scope

**Source:** `in-scope-flows.md` (P3c), `capabilities-map.md` (P3b)
**Owner:** SA drafts; PM reviews for completeness.

### 3.1 In Scope

List the flows and capability domains included in this engagement. Be specific — reference the flows by name. For each flow, one sentence on what we are building or changing.

### 3.2 Out of Scope

Explicit list of what is not included. This section prevents scope creep and sets client expectations. Include adjacent capabilities that came up in discovery but were deprioritised.

### 3.3 Key Assumptions

The assumptions this proposal rests on. If any assumption is wrong, the scope or estimate may change.
_Source:_ `known-unknowns.md` (P3d), estimation uncertainty drivers from `estimation-final.md`

---

## 4. Our Approach

**Source:** `architecture.md` section 1 — Architectural approach (P4b-i)
**Owner:** SA drafts.

### 4.1 Architectural Approach

The core idea behind the solution — what we are building and why this is the right approach for this client at this budget. Platform selection rationale if relevant (e.g. why Thought Machine Vault vs. Mambu).

Plain language — technical details belong in Section 6. This section should be readable by a non-technical executive.

### 4.2 Guiding Principles

3–5 principles that shaped the solution (e.g. "phase delivery to reduce risk", "reuse before build", "regulatory compliance built in from day one, not added later"). Connect each to a client constraint or goal from Section 2.

### 4.3 Vacuumlabs Reuse & Accelerators

Which VL assets and past-deal patterns we are bringing to this engagement and how they reduce risk or timeline.
_Source:_ `vl-assets.md` (P4a), reuse section of `architecture.md`

---

## 5. Business Architecture

**Sources:** `capabilities-map.md`, `in-scope-flows.md`, `business-architecture.md` (P3e)
**Owner:** PO drafts; SA reviews; PM owns RACI accuracy.

_Run `/p3e` to generate the source material for this section._

### 5.1 Stakeholders & Actors

* Stakeholder table: name, role/group, primary interest in the engagement, decision authority (Sponsor / Approver / Consulted / Informed)
* Actor list: human actors (customer, branch agent, ops user, admin) and system actors (existing CBS, KYC vendor, payment scheme, etc.)
* Diagram: simple actor/system context view (1 page)

### 5.2 Value Streams

* 3–5 end-to-end value streams scoping the engagement (e.g. "Onboard a retail customer", "Open a savings account", "Process a card transaction dispute")
* For each stream: stakeholder receiving value, stages, capabilities used per stage, value delivered
* Summary table: one row per stream; one half-page diagram for the headline stream
* Explicitly cross-reference §3 In-Scope Flows

### 5.3 Responsibility Split (RACI)

* At workstream / capability cluster level — not task level
* Parties: Bank, Vacuumlabs, Platform Vendor (e.g. Thought Machine), other SIs/3rd parties, named consultancies
* Coverage: solution design, platform configuration, integration build, data migration, testing, regulatory sign-off, production cutover, hypercare, post-go-live BAU
* Flag any responsibility ambiguities that Discovery must resolve — this is both honest and protects VL

### 5.4 Operating Model Impact _(optional — transformational deals only)_

* Which client teams are affected and how their work changes
* New capabilities/roles the client needs to build internally (e.g. "product configuration team for TM Vault Contracts")
* Training and change management implications — headline level only
* Link to client-side commitments in §8.3

### 5.5 Outcomes & Success Measures

* 3–5 measurable business outcomes tied to capabilities
* Format: capability change → operational metric → business outcome
* Example: "Product config in TM Vault → time-to-launch new product from 6 months to 2 weeks → ability to compete with neobanks on speed"
* Pulls from §2.4 (What Success Looks Like) and makes it traceable

---

## 6. Technical Architecture

**Source:** `architecture.md` (P4b-i and P4b-ii)
**Owner:** SA owns entirely.

### 6.1 System Components

Key components of the solution, their responsibilities, and how they map to the in-scope flows. Use a table or structured list. Reference the architecture diagram.

### 6.2 External Integrations

Third parties and existing client systems that connect to what we are building. For each: what it is, direction of data flow, integration pattern, and any notable constraint or risk.

### 6.3 Technology Stack

Chosen technologies per layer (backend, frontend, data, infrastructure) with a brief rationale for each choice.

### 6.4 Infrastructure

Deployment model, cloud provider and region, environments, key infrastructure components. Call out any data residency or regulatory constraints that shaped these choices.

### 6.5 Architecture Diagram

Embed or reference the C4 Level 1 (System Context) and Level 2 (Container) diagrams.

---

## 7. Delivery Plan

**Source:** `phasing.md` (P5b), `epics.md` (P5a)
**Owner:** SA drafts; PM reviews and owns timeline commitments.

### 7.1 Phases Overview

Summary table of all phases. Every plan must include UAT, go-live, hypercare, and handover as explicit phases — do not omit them.

| Phase | Scope summary | Duration | Team size | Client value delivered |
| --- | --- | --- | --- | --- |
| P0 — Discovery _(if applicable)_ | ... | X weeks | N FTEs | ... |
| P1 — ... | ... | X months | N FTEs | ... |
| UAT | Client acceptance testing across all in-scope flows | X weeks | N FTEs | Client signs off on delivery |
| Go-live & Release | Production cutover, release activities | X days/weeks | N FTEs | System live in production |
| Hypercare | Intensive post-go-live support; rapid response to production issues | 2–4 weeks | N FTEs | Stable production operation |
| Handover | Knowledge transfer, documentation handover, BAU transition | X weeks | N FTEs | Client team self-sufficient |

### 7.2 Phase Detail

For each phase (one subsection per phase):

* Epics and deliverables in scope
* What is explicitly not in scope for this phase
* Team composition (roles and FTEs)
* Key milestones
* Dependencies — what must be true before this phase starts
* What the client can do or demonstrate at the end of this phase

### 7.3 Timeline

Visual or tabular timeline from project start to handover completion. Highlight: start date, first live product, go-live, end of hypercare.

---

## 8. Team & Ways of Working

**Source:** `phasing.md` team composition sections (P5b)
**Owner:** PM drafts; SA confirms technical roles.

### 8.1 Proposed Team

Core team composition for the primary delivery phases. For each role: title, responsibilities, FTE allocation.

### 8.2 Ways of Working

How VL will work with the client team: sprint cadence, ceremonies, reporting, escalation path. Keep this brief — 1 paragraph.

### 8.3 Client-Side Commitments

What we need from the client for delivery to succeed: access to stakeholders, timely decisions, system access, participation in key ceremonies. Specifically for the closing phases: dedicated UAT resources and timelines, go/no-go decision authority, named handover recipients for knowledge transfer.

---

## 9. Estimate

**Source:** `estimation-final.md` (P5)
**Owner:** SA owns numbers; Sales owns commercial framing and sign-off.

### 9.1 Estimate Summary

| | |
| --- | --- |
| **Total effort** | X–Y man-days |
| **Indicative cost** | €X–€Y |

### 9.2 Estimate by Phase

Phase-by-phase cost breakdown (effort and indicative cost per phase), including UAT, go-live, hypercare, and handover.

| Phase | Effort (man-days) | Indicative cost |
| --- | --- | --- |
| P0 — Discovery _(if applicable)_ | X–Y | €X–€Y |
| P1 — ... | X–Y | €X–€Y |
| UAT | X–Y | €X–€Y |
| Go-live & Release | X–Y | €X–€Y |
| Hypercare | X–Y | €X–€Y |
| Handover | X–Y | €X–€Y |
| **Total** | **X–Y** | **€X–€Y** |

### 9.3 Key Assumptions

The 3–5 assumptions the estimate rests on. Be direct about what would cause the number to move up or down.

### 9.4 What Is Not Included

Items excluded from the estimate that the client may expect to be included (e.g. third-party licensing, infrastructure run costs, client-side change management).

### 9.5 Post Go-Live

Indicative maintenance and support model and costs, if relevant.

---

## 10. Risks & Mitigations

**Source:** Architecture risks from `architecture.md` section 8, `known-unknowns.md` (P3d)
**Owner:** SA drafts; PM reviews for delivery risks.

### 10.1 Risk Table

| Risk | Likelihood | Impact | Mitigation |
| --- | --- | --- | --- |
| ... | H/M/L | H/M/L | ... |

Cover: technical risks, delivery risks, commercial risks, regulatory/compliance risks.

### 10.2 Open Questions

Open questions that are material to the engagement scope, timeline, or commercial terms should be explicitly surfaced in the proposal. This signals transparency to the client and protects Vacuumlabs from scope creep assumptions. The section should capture questions that remain unanswered at the time of submission and must be resolved during or before Discovery.

---

## 11. Why Vacuumlabs

**Owner:** Sales drafts; SA provides evidence points.

### 11.1 Relevant Experience

2–3 past engagements directly relevant to this client's challenge. For each: client/domain (anonymised if needed), what we built, the outcome.

### 11.2 Domain & Technical Expertise

Specific depth VL brings: platform expertise (e.g. Thought Machine Vault depth), domain knowledge (lending, payments, compliance), AI-native delivery.

### 11.3 How We Are Different

Honest differentiators vs. the likely competition. What VL can credibly claim that larger SIs or platform vendors cannot easily match.
_Source:_ `competitive-positioning.md` (P2c)

---

## 12. Next Steps

**Owner:** Sales.

* Proposed timeline to contract
* What we need from the client to proceed (decisions, sign-off, access)
* VL contact for questions and follow-up

---

## Appendices (as needed)

* A — Detailed work package estimates (epic-level breakdown)
* B — Architecture diagrams (full size)
* C — Team CVs / profiles
* D — Reference client case studies
* E — Compliance matrix (T3 only)

---

## T1 Deviation

For ballpark engagements, replace Sections 3–8 with a single **Scope, Approach & Estimate** page covering:

* Scope sketch (what's in, what's assumed out)
* Approach in 2–3 sentences
* Rough effort range with explicit assumptions
* One call-out on the biggest uncertainty that could move the number

Sections 2, 9, 10, 11 still apply in shortened form.
