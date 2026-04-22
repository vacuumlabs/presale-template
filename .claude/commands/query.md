---
description: Answer a question using the wiki; offer to refile the answer as a new wiki page
argument-hint: <question>
---

# Query the wiki

Question: **$ARGUMENTS**

Follow this workflow.

## 1. Read the index first

Open [`index.md`](../../index.md). It's the map. Identify which wiki pages are likely relevant before you do any broader search.

If the index seems stale or incomplete relative to the question, mention it — this is a signal for `/lint`.

## 2. Drill into wiki pages

Read the wiki pages you identified. Follow their cross-links. Pull in raw sources (under `client-inputs/` or `team-inputs/`) only when the wiki page's synthesis is insufficient.

Prefer the wiki over the raw layer when they agree. Treat disagreement as a contradiction to flag (see step 5).

## 3. Synthesise an answer

Produce the answer with **explicit citations**. Every factual claim links to at least one wiki page or raw source via relative markdown link.

Be direct about gaps: if the wiki does not contain the information needed, say so — do not infer. Name the specific source that would fill the gap (e.g. *"we don't have the signed DPA in `contract/clauses/`; ask Legal for the latest version"*).

## 4. Offer to refile

If the answer is likely to be re-asked, ask the user whether to save it as a new wiki page:

- Propose the target folder (see [`AGENTS.md`](../../AGENTS.md) routing).
- Propose the filename and metadata.
- On confirmation, write the page and update [`index.md`](../../index.md).

This is how the wiki compounds — query outputs become wiki pages.

## 5. Flag anything you noticed

If while answering you spotted a contradiction, an orphan page, or a missing wiki page that would have made this query faster, raise it. For contradictions, offer to append to [`contradictions.md`](../../contradictions.md).

## 6. Append to log.md

Append to [`log.md`](../../log.md):

```
## [YYYY-MM-DD HH:MM] query | <one-line summary of question>

<If refiled, link to the new page.>
```
