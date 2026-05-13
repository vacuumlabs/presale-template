# T3 — ARB Pre-Sale Review Submission

_This is the 10-section template for Architecture Review Board pre-sale submissions. Complete all sections before the ARB session. Distribute to ARB members at least 24 hours before the session._

_Run `/p6` to generate a first draft of this document from the accumulated project context. SA reviews and refines before the ARB session._

**Usage:** Copy this file to `technical-architecture/arb-records/YYYY-MM-DD-arb-submission.md` and fill it in (or use `/p6` to draft it). After the ARB session, record feedback in the final section and save the reviewed version to Confluence ARB space.

---

**Client:** \__________\_
**Deal type:** T1 / T2 / T3
**SA:** \__________\_
**Prepared by:** \__________\_
**ARB session date:** \__________\_
**Review type:** Pre-Sale Architecture Review

---

## Section 1 — Review Context

* What decision does this ARB review need to support?
* What is the deadline and why?
* Is this a first review or a follow-up?
* What specific questions does the SA want the ARB to focus on?

---

## Section 2 — Business Context & Goals

* Who is the client and what do they do?
* What are they trying to achieve with this engagement?
* What does success look like in business terms?
* Engagement tier and contract model (T&M / FSFP)
* Budget signal: Hard / Soft / Unknown — amount

---

## Section 3 — Assumptions & Open Questions

List all material assumptions the architecture rests on. For each:

* State the assumption
* State what happens if the assumption is wrong
* Note whether it has been confirmed or is unverified

List all significant open questions. For each:

* State the question
* State the impact if it cannot be answered before delivery begins

---

## Section 4 — Business View

* Key business processes this solution supports (from capabilities map)
* In-scope flows: key happy paths, edge cases, current pain points
* Key user types and their interactions
* Scope boundary: what is in and out of scope

---

## Section 5 — Technical Architecture Overview

* Proposed architecture at C4 Level 1–2
* Include or reference architecture diagrams (repo path or GDrive location)
* Key components and their responsibilities
* Key integration points: what integrates, how, complexity
* Key data flows for the most important business processes
* Architectural decisions made and alternatives considered

---

## Section 6 — Risks, Security & Compliance

For each risk:

* Risk description
* Likelihood: High / Medium / Low
* Impact: High / Medium / Low
* Proposed mitigation
* Residual risk after mitigation

Include: technical risks, commercial risks, delivery risks, regulatory/compliance risks, security considerations.

---

## Section 7 — Reuse & Alignment

* Which VL assets, accelerators, or reference architectures are applicable? (from `vl-assets.md`)
* Which confirmed reuse items are included in the estimate?
* Past deals with similar scope that should inform this estimate
* Any conflicts with existing VL engagements

---

## Section 8 — Estimate Drivers

* Top 5 drivers of effort
* What would cause the estimate to increase materially?
* What would cause it to decrease?
* Development effort range (from `estimation-dev.md`): [min]–[max] man-days
* Overhead allocations (from `estimation-overhead.md`): PM, PO, QA, design, infra by phase
* Combined estimate (from `estimation-final.md`):

    * Effort: [min]–[max] man-days
    * Indicative cost: €[min]–€[max]

* Key assumptions the estimate rests on
* Budget reconciliation: how the estimate compares to the budget signal (Hard / Soft), or the three-option scenarios produced for Unknown signal

---

## Section 9 — Summary & Requested Feedback

* One-paragraph summary of the proposed approach and why it is the right recommendation
* What specific feedback does the SA need from the ARB?

---

_Sections 10+ are for Delivery Architecture Reviews only — not required for Pre-Sale reviews._

---

_After the ARB session, document here:_

**ARB feedback:**

**Decisions made:**

**Actions required before proposal is sent:**

**Confluence record:** \[URL\]
