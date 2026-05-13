---
description: Pull the latest schema-layer updates from the vacuumlabs/project-template repository
---

# Sync template schema

Pulls the latest schema-layer updates (hooks, slash commands, subagents, routing docs, merge drivers) from `vacuumlabs/project-template` into this repo. **Wiki content is never touched** — `client-inputs/`, `team-inputs/`, wiki folders, `log.md`, `index.md`, `decisions.md`, `contradictions.md`, and `.claude/settings.local.json` are all preserved as-is.

Follow this workflow exactly. Do not skip steps.

## 0. Sync with origin

Before doing anything else, run `git pull --ff-only --quiet` to pick up any pushes from teammates. If the pull fails (non-fast-forward, conflicts, network error), stop and ask the user to resolve before continuing.

## 1. Guard the working tree

Run `git status --porcelain`. If it returns anything, **abort** with:

> *"You have uncommitted changes. Commit or stash them first, then re-run `/sync-template`."*

This prevents sync-applied edits from being accidentally rolled up with the user's in-flight work.

## 2. Fetch the remote manifest

Read `sync-manifest.json` at the repo root to find `template_repo` and `template_branch`. Fetch the remote manifest:

```bash
curl -sfL "https://raw.githubusercontent.com/<template_repo>/<template_branch>/sync-manifest.json"
```

If curl fails (network error, 404, 403), report the error and abort. A 403/404 on a private template repo means the user needs `gh auth login` first — tell them.

## 3. Compare versions

Read local `sync-manifest.json` and compare `version` with remote `version`.

- **Equal:** report *"You are on template v<X.Y.Z>, same as the template. Nothing to sync."* and exit.
- **Different:** continue.

## 4. Diff every file in the remote manifest

For each path in the remote `files` array:

- Fetch the remote contents: `curl -sfL "https://raw.githubusercontent.com/<template_repo>/<template_branch>/<path>"`.
- Read the local file (if it exists).
- Classify the pair into one of:
  - `identical` — no action needed.
  - `changed` — remote differs from local; will overwrite on confirm.
  - `new` — file exists in remote but not locally; will create on confirm.

Also compute files to delete: any path in the **local** manifest's `files` array that is not in the **remote** manifest's `files` array. Those will be `git rm`'d on confirm.

## 5. Present the summary

Show the user:

- Version bump: `<local_version>` → `<remote_version>`
- Counts: `<N> changed, <M> new, <K> to delete`
- For each `changed` file: the path plus a compact diff stat (lines added / removed).
- For each `new` or `to delete` file: the path only.
- **Flag prominently** any file where the local version appears to have been customised locally (substantially diverged from the prior template version). For these, recommend reviewing the diff carefully before overwriting.

Also note: **External tools listed in `sync-manifest.json` under `external_tools` are candidates — install separately if needed. This command does not install them.**

Ask the user to choose:
- **Apply all** — proceed with everything.
- **Pick per file** — step through each changed file and confirm or skip.
- **Cancel** — abort without writing anything.

## 6. Apply the accepted changes

For each accepted change:

- `changed` / `new`: write the fetched remote contents to the local path.
- `to delete`: `git rm <path>`.

Finally, overwrite the local `sync-manifest.json` with the remote one (this is how the local version marker advances).

## 7. Commit

Stage and commit:

```
sync: schema → template v<remote_version>

<one-line summary: "<N> files updated, <M> added, <K> removed">
```

The Stop hook will push.

## 8. Report

Return a structured summary:

- **Version:** `<local>` → `<remote>`
- **Updated:** `<list of paths>`
- **Added:** `<list of paths>`
- **Removed:** `<list of paths>`
- **Skipped** (if user picked per-file): `<list of paths>`

Do **not** append to `log.md`. The commit history and `sync-manifest.json` version are the durable record; `log.md` is reserved for wiki-content events.

## Notes

- Files NOT in the manifest are never read, written, or deleted by this command.
- If the remote manifest adds new paths, the user sees them in the `new` bucket and decides whether to accept.
- If curl is unavailable on the user's system, this command won't work — report and suggest installing it.
- For private template repos: replace the `curl -sfL` calls with `gh api repos/<template_repo>/contents/<path>?ref=<template_branch> --jq .content | base64 -d`. Detect the private-repo case when the first curl returns 404 from an otherwise-reachable host.
