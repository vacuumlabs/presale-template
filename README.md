# Project Template

A template repository for running an AI-assisted SDLC against a real client engagement. It is the project's knowledge base — a structured home for the contract, deal context, architecture, scope, product and project management artefacts, and raw client inputs — queryable by Claude (or any LLM) and by humans.

See [`AGENTS.md`](AGENTS.md) for the routing map of what lives where and the conventions for authoring artefacts.

## Contents

- [How to use this repo](#how-to-use-this-repo)
- [Automating the engagement](#automating-the-engagement)
- [Setup for non-technical users](#setup-for-non-technical-users)
- [Why we are doing this](#why-we-are-doing-this)
- [Why a GitHub repository (and not a Claude project)](#why-a-github-repository-and-not-a-claude-project)

## How to use this repo

Each client engagement gets its own repository, created from this template via GitHub's **Use this template** button. Once created, follow the kickoff checklist, then use the repo throughout delivery, and archive it at close-out.

### Kickoff checklist

- [ ] Rename the new repo to `<client>-engagement` (or the current team convention) and update its description.
- [ ] Fill in [`contract/engagement.md`](contract/engagement.md) with client, project code, start date, engagement type (FSFP / T&M), delivery lead, and primary stakeholders.
- [ ] Configure GitHub access — invite the delivery team and set branch protection on `main` if desired.

### Running the engagement

- Add or update artefacts in the directory that matches their topic; see [`AGENTS.md`](AGENTS.md) for routing and conventions.
- Claude is the primary interface — it reads and writes the repo directly, using `AGENTS.md` to route questions and ground its answers in the files.
- Humans can edit via the GitHub web UI, via `git`, or by asking Claude to make the change.

### At close-out

- Archive the repo on GitHub when delivery finishes. History stays intact and read-only.

## Automating the engagement

Every engagement has repetitive work — transcripts to file, status to circulate, inputs to synthesise. Claude Code can automate most of it; what's worth automating depends on *your* bottlenecks, not a predetermined list. Notice where your time goes, then ask Claude to take it over.

**How to start:** describe the job in plain English — *"every morning, pull yesterday's meeting transcripts into `client-inputs/`"* — and Claude will set it up. No need to know how.

**A few ideas to spark your own** (steal what fits, ignore the rest):

- Daily upload of meeting notes and emails into `client-inputs/` so inputs don't stay stuck in someone's inbox.
- Synthesis of raw transcripts into decisions and action items in the right curated folder.
- Daily, weekly, or post-meeting digests of what changed — for yourself, the delivery lead, or a Slack channel.

The best automations are the ones you notice yourself. Keep a running list as you go.

## Setup for non-technical users

This repo is designed to be driven through [Claude Code](https://claude.com/claude-code). You do not need to know git — Claude runs all the git commands for you. You do need a one-time setup so Claude can talk to GitHub on your behalf, and a working mental model of what "commit" and "push" actually do so you can reason about where your files have travelled.

### 1. Set up passwordless SSH access to GitHub (one-time per laptop)

GitHub needs to recognise your machine without asking for a password every time Claude pushes or pulls. Takes about five minutes.

1. **Check whether you already have a key.** Ask Claude Code: *"Do I already have an SSH key for GitHub?"* Claude looks in `~/.ssh/` for a file like `id_ed25519.pub` or `id_rsa.pub`. If one exists, skip to step 4.
2. **Create a new key.** Ask Claude Code: *"Generate an SSH key for GitHub using my work email."* Claude will run `ssh-keygen -t ed25519 -C "you@vacuumlabs.com"`. Pressing Enter twice (empty passphrase) is fine for a work laptop; pick one if you want extra security — macOS Keychain will remember it either way.
3. **Let macOS Keychain remember the key.** Ask Claude Code: *"Add my SSH key to the macOS keychain so I don't get prompted."* That runs `ssh-add --apple-use-keychain ~/.ssh/id_ed25519`.
4. **Copy your public key.** Ask Claude Code: *"Print my SSH public key and copy it to the clipboard."* Claude will run `pbcopy < ~/.ssh/id_ed25519.pub`.
5. **Add the key to GitHub.** Go to [github.com/settings/ssh/new](https://github.com/settings/ssh/new), paste the key, name it (e.g. *"MacBook — work"*), and save.
6. **Test it.** Ask Claude Code: *"Check that my SSH connection to GitHub works."* Claude runs `ssh -T git@github.com` — you should see `Hi <username>! You've successfully authenticated…`.

After this, every `git push` / `git pull` just works — no passwords, no prompts.

### 2. Get a project onto your laptop

1. On GitHub, open the repository page and click **Code → SSH**. Copy the URL (looks like `git@github.com:vacuumlabs/<repo>.git`).
2. Open Claude Code in the folder where you want the project to live (e.g. `~/work/`).
3. Ask Claude Code: *"Clone `git@github.com:vacuumlabs/<repo>.git` into this folder and open it."*
4. Claude runs `git clone` and `cd`s into the new directory. You can now ask questions about the project.

To start a brand-new project from this template: *"Create a new repo from the `vacuumlabs/project-template` template named `<client>-engagement`, clone it here, and walk me through the kickoff checklist."*

### 3. What "commit" and "push" actually mean

Think of the repo as having **three places** a file can live:

| Place | Where it lives | Who can see it |
| --- | --- | --- |
| **Working copy** | The files open on your laptop | Only you, right now |
| **Commit (local history)** | A saved snapshot stored in `.git/` on your laptop | Only you, until you push |
| **GitHub (remote)** | The copy on GitHub's servers | Everyone with repo access |

The lifecycle is always: **edit → commit → push**.

- **Edit** — you (or Claude) change a file. Nothing is saved to history yet. If your laptop dies right now, the change is gone.
- **Commit** — Claude runs `git commit`. The change is saved as a labelled snapshot in the project's history *on your laptop*. It is still not on GitHub.
- **Push** — Claude runs `git push`. All new commits travel from your laptop to GitHub, where teammates see them.

You will usually ask Claude to do both together: *"commit and push this"*.

### 4. What can and cannot be reverted

| Situation | Can it be undone? | How |
| --- | --- | --- |
| You edited a file but haven't committed yet | Yes, cleanly | Ask: *"undo my uncommitted changes to `<file>`"* |
| You committed but haven't pushed | Yes, cleanly | Ask: *"revert the last commit"* or *"remove `<file>` from the last commit"* |
| You pushed to GitHub | Yes, but the original stays in history | Ask: *"revert the last commit and push the revert"* — Claude creates a new commit that undoes the old one. The original is still visible in history (this is a feature, not a bug: it's the audit trail) |
| You committed a secret (password, API key, token) | **The secret is now permanently in history.** Rotate it immediately | Rotate/revoke the secret first, then ask Claude to remove the file. For anything sensitive, assume it's been seen |

**Golden rule:** the further a change has travelled (working copy → commit → push), the more work it takes to undo. Catching mistakes before `push` is always easier.

### 5. How to get an unwanted file out of a commit

**Before you pushed** — ask Claude Code: *"I accidentally committed `<file>`. Remove it from the last commit but keep my other changes."* Claude will unstage it, amend the commit, and confirm what changed.

**After you pushed** — ask Claude Code: *"I pushed `<file>` by mistake. Remove it from the repo going forward."* Claude will commit a deletion and push it. The old version still exists in history (see the table above); if the file contained a secret, treat it as leaked and rotate it.

### 6. Safety rails this repo ships with

Two automations in [`.claude/settings.json`](.claude/settings.json) keep you out of trouble:

- **Auto-sync before every action.** Before Claude edits files or runs shell commands, it silently runs `git fetch` + `git pull --ff-only` so your local copy stays current with GitHub.
- **Force-push to `master` is blocked.** Claude will refuse to force-push the `master` branch — a force-push can silently delete teammates' work. If you genuinely need one (you almost never do), open `.claude/settings.json` and temporarily remove the guard.

## Why we are doing this

Client projects generate a lot of context that today lives scattered across Slack, Drive, email, decks, and individual heads. When that context isn't in one place:

- onboarding a new person (or an AI agent) is slow and lossy,
- decisions get re-litigated because nobody can find the previous answer,
- questions from the client take hours of archaeology to answer,
- critical details (a contract clause, an architectural assumption) silently go missing.

Consolidating project knowledge into a single, well-structured, version-controlled place fixes all of that. It also makes the knowledge *queryable* — both humans and LLMs can navigate a predictable folder layout and get grounded answers instead of guesses.

## Why a GitHub repository (and not a Claude project)

We previously kept this material inside a Claude project. Moving it into a Git repository gives us several properties a Claude project can't:

- **Resilience to Claude outages.** The files live outside Claude. If Claude is down or a workspace breaks, the knowledge is still there, still readable, still editable with any tool.
- **Single folder on disk.** Everything the project needs is in one place — clonable, backupable, greppable — instead of split between a Claude workspace and wherever the "real" files live.
- **Full change history.** Git records who changed what and when. Every artefact has an audit trail, blame, and the ability to time-travel to a previous state.
