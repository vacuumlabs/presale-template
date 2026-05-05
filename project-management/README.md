# project-management/

Roadmaps, timelines, milestones, capacity planning, status reports, project assembly (client-facing outputs and presentations), debriefs, post-mortems. Also the scope and estimates wiki (work breakdown, assumptions, change requests) — scope sits here because it is managed alongside risks, status, and capacity.

## Typical wiki pages

### Project management

- `roadmap.md` — current roadmap with milestones
- `milestones.md` — milestones with dates, exit criteria, status
- `risks.md` — risk register with owner, likelihood, impact, mitigation
- `capacity.md` — team composition, allocation, ramp plan
- `status/YYYY-MM-DD-status.md` — weekly status snapshots
- `debriefs/<topic>.md` — post-milestone or post-mortem write-ups
- `assembly/<name>.md` — client-facing deliverables (decks, reports, presentations) plus their matching `.md` summary files per the binary-companion rule

### Scope & estimates

- `work-breakdown.md` — the tree of epics / features / stories
- `epics/<name>.md` — one page per epic (goal, acceptance criteria, dependencies, risk, MD estimate range)
- `assumptions.md` — append-log of assumptions behind the estimate. Every assumption is a latent change request
- `change-requests.md` — formal change requests, their status, commercial impact
- `estimate-methodology.md` — how we estimated (T-shirt, three-point, reference class, …) so the numbers are defensible
- `out-of-scope.md` — explicit list of things we are *not* doing, with rationale
- `open-questions.md` — unresolved product / scope questions (usually PO-owned; promote to `../contradictions.md` when they conflict with existing wiki content)

`/lint` reports land in [`lint-reports/`](./lint-reports/) as `YYYY-MM-DD-lint-report.md`.

## Index

<!-- Claude maintains this list -->

_(empty)_
