---
description: Answer a question using the wiki; offer to refile the answer as a new wiki page
argument-hint: <question>
---

# Query the wiki

Question: **$ARGUMENTS**

Follow this workflow.

## 0. Sync with origin

Before reading anything, run `git pull --ff-only --quiet` to pick up any pushes from teammates. If the pull fails (non-fast-forward, conflicts, network error), stop and ask the user to resolve before continuing.

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
- On confirmation, write the page and update **both** the root [`index.md`](../../index.md) **and** the parent folder's `README.md` `## Index` section (per the two-level index rule in [`AGENTS.md`](../../AGENTS.md)).

This is how the wiki compounds — query outputs become wiki pages.

## 5. Persist decisions and contradictions (gated on user confirmation)

If while answering you surfaced:

- **A decision worth recording** — present it to the user and ask whether to save it. On confirmation, append to [`decisions.md`](../../decisions.md) **and** append a `decision` entry to [`log.md`](../../log.md).
- **A contradiction worth flagging** — present both sides to the user and ask whether to record it. On confirmation, append to [`contradictions.md`](../../contradictions.md) **and** append a `contradiction` entry to [`log.md`](../../log.md).

Log entry format:

```
## [YYYY-MM-DD HH:MM] decision | <one-line summary>

<1–2 sentence context; link to the relevant decisions.md / contradictions.md block.>
```

Also raise (but don't persist) any orphan pages or missing wiki pages you noticed — these are signals for `/lint`.

## 6. No log entry for routine queries

A pure read-only query does **not** touch `log.md`. The log tracks material knowledge-state changes, not every lookup. If the user explicitly asks *"log this query"* (rare), use the `query` verb with the same entry format as above.
