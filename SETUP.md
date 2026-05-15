# Setup

One-time-per-laptop bootstrap to drive this repo through Claude Code in the Claude desktop app — install, GitHub access, clone, commit/push, undo. All roles do this once.

Why the Claude app and not the terminal CLI or web app: the desktop app gives Claude Code the project folder, handles git for you, and surfaces its workspace as a panel beside your other tools. No git or terminal knowledge required.

## 1. Install the Claude app (with Claude Code)

Download the Claude desktop app from [claude.ai/download](https://claude.ai/download) and sign in with your Anthropic account. Once installed, enable **Claude Code** inside the app — [claude.com/claude-code](https://claude.com/claude-code) has the up-to-date per-OS steps. After enabling, Claude Code appears as a panel inside the app; you'll point it at a project folder in step 3.

From here on, every instruction that says *"ask Claude Code …"* means: type the quoted prompt into the Claude Code panel in the Claude app.

## 2. Install required skills and plugins (one-time per laptop)

Two additions make Claude Code aware of Vacuumlabs-specific process knowledge and general-purpose engineering skills.

### Vacuumlabs Presale Guide skill

The `vacuumlabs-presale-guide` skill is bundled in this repo. Copy it to your user skills folder so Claude Code picks it up in every session:

**Mac/Linux:**
```
cp .claude/skills/vacuumlabs-presale-guide.skill ~/.claude/skills/
```

**Windows (PowerShell):**
```
Copy-Item .claude\skills\vacuumlabs-presale-guide.skill "$env:USERPROFILE\.claude\skills\"
```

Restart Claude Code after copying. Once installed, Claude Code automatically invokes the skill when you ask process questions about the presale.

### Superpowers plugin (recommended)

Superpowers adds general engineering skills (brainstorming, systematic debugging, TDD, code review) that work alongside the presale commands. Install it once via Claude Code:

```
/plugin marketplace add anthropics/claude-plugins-official
/plugin install superpowers@claude-plugins-official
```

Both steps require an active internet connection. The plugin persists across all your projects — install it once and it's always available.

## 3. Passwordless GitHub access (one-time per laptop)

Takes about five minutes. Ask Claude Code for each step — **follow Claude's instructions as they appear, then come back here for the next step**:

1. *"Do I already have an SSH key for GitHub?"* — Claude checks `~/.ssh/`. If one exists, skip to step 5.
2. *"Generate an SSH key using my work email."* — empty passphrase is fine on a work laptop.
3. *"Add my SSH key to the macOS keychain."*
4. *"Print my SSH public key and copy it to the clipboard."*
5. At [github.com/settings/ssh/new](https://github.com/settings/ssh/new), paste the key, give it a name (e.g. `Work MacBook`), and save.
6. *"Test my SSH connection to GitHub."* — expect `Hi <username>! You've successfully authenticated…`.

> **Note:** When you run step 2, Claude may itself suggest going to GitHub to paste the key — that's step 5. Follow along, then return here and continue from step 6 to verify the connection.

After this, every `push` / `pull` just works. If you see `Permission denied (publickey)` at any point, ask Claude: *"My SSH connection to GitHub is failing — diagnose it."*

## 4. Get a project onto your laptop

On GitHub, open the repo → **Code → SSH**, and copy the URL. Then, in the Claude app:

1. Click the **`+`** button (top-left of the Claude Code panel) to start a new session.
2. When prompted to choose a folder, navigate to your working directory (e.g. `~/work/`) and select it.
3. Ask: *"Clone `git@github.com:vacuumlabs/<repo>.git` here and open it."*
4. Once cloning finishes, click **`+`** again, navigate into the newly-cloned folder (e.g. `~/work/project-qcp-kb/`), and select it. This sets it as the active working context.

New project from this template: on GitHub, open [`vacuumlabs/project-template`](https://github.com/vacuumlabs/project-template) → **Fork** → name the fork `<client>-engagement`. Then in the Claude app: *"Clone `git@github.com:vacuumlabs/<client>-engagement.git` here and walk me through the kickoff checklist."*

> **Each time you open a new chat**, check the folder shown in the top-left of the Claude Code panel — it should show the project repo, not a previous project. If it's wrong, click `+` and re-select the correct folder before asking anything.

## 5. Commit and push, in plain English

A file lives in one of three places:

| Place | Where | Who sees it |
| --- | --- | --- |
| **Working copy** | Files open on your laptop | Only you, right now |
| **Commit** | Snapshot in `.git/` on your laptop | Only you, until you push |
| **GitHub** | Server copy | Everyone with repo access |

Lifecycle: **edit → commit → push**. Usually: *"commit and push this."*

## 6. What can be undone

| Situation | Undoable? | Ask Claude |
| --- | --- | --- |
| Edited, not committed | Yes, cleanly | *"Undo my uncommitted changes to `<file>`."* |
| Committed, not pushed | Yes, cleanly | *"Revert the last commit"* / *"Remove `<file>` from the last commit."* |
| Push rejected (someone pushed first) | Yes | *"My push was rejected — pull and push my changes."* |
| Pushed | Yes, but the original stays in history | *"Revert the last commit and push the revert."* Claude adds a new commit undoing the old one; history is preserved (feature, not bug). |
| Committed a secret | **Permanent in history.** Rotate first. | Rotate the secret, then ask Claude to remove the file. Assume it's been seen. |

The underlying principle: the further a change has travelled (working copy → commit → pushed), the more work it takes to undo. Catching mistakes before `push` is always cheaper.

## 7. Safety rails already in place

Two hooks live in [`.claude/settings.json`](.claude/settings.json) — auto-sync before every action, and force-push to `master` blocked. Full description in [`AGENTS.md` § "Automations configured in this repo"](AGENTS.md#automations-configured-in-this-repo).
