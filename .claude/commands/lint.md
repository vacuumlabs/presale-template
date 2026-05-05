---
description: Health-check the wiki — contradictions, stale pages, orphans, index drift, missing wiki pages
---

# Lint the wiki

Produce a report at `project-management/lint-reports/YYYY-MM-DD-lint-report.md`. Do not silently "fix" issues — list them and ask the user before acting.

Run the following checks.

## 0. Sync with origin

Before scanning anything, run `git pull --ff-only --quiet` to pick up any pushes from teammates. If the pull fails (non-fast-forward, conflicts, network error), stop and ask the user to resolve before continuing.

## 1. Unresolved contradictions

Scan [`contradictions.md`](../../contradictions.md) for entries whose `Resolution:` is `?`. List each with first-flagged date and owner.

## 2. Stale pages

List every wiki page whose `last_updated` metadata is more than 14 days old while engagement status is still active. For each, suggest a one-line reason it may be stale (new raw source landed, dependency page changed, etc.).

## 3. Orphan pages

Wiki pages with no inbound cross-links from any other page in the wiki. These are either genuinely isolated (probably a mistake) or never got integrated.

## 4. Missing wiki pages

Entities / concepts mentioned 3+ times across wiki pages without a dedicated wiki page of their own. Candidate promotions.

## 5. Index drift

Wiki pages that exist on disk but are missing from [`index.md`](../../index.md), or missing from their folder's README. Also the reverse: index entries pointing at files that don't exist.

## 6. Provenance holes

Wiki pages with:

- Empty `sources:` metadata, or
- `sources:` entries pointing at files that no longer exist at that path.

## 7. Binary companion gaps

Any non-markdown file under `client-inputs/` or `team-inputs/` without a matching `.md` summary.

## 8. TODO audit

Open `- [ ]` checkboxes across the wiki:

- With a due date in metadata or body that has passed.
- Without a due date and older than 21 days (use git blame).

## 9. Metadata validity

Wiki pages missing required metadata fields (`type`, `title`, `sources`, `last_updated`, `status`).

## Output

Write the report to `project-management/lint-reports/YYYY-MM-DD-lint-report.md` with one section per check above. Each finding should be one bullet, with a link to the affected page.

At the top of the report, add a one-paragraph summary: *N stale, M orphans, P contradictions unresolved, Q missing wiki pages.*

Append to [`log.md`](../../log.md):

```
## [YYYY-MM-DD HH:MM] lint | <summary headline>

Report: [link to the YYYY-MM-DD-lint-report.md]
```

Then ask the user which findings to action now vs. defer.
