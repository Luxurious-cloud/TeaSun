# ClaudeKit (CK Engineer) setup

Steps to install ClaudeKit for this repository. **These have to be run on a local machine** —
see [Why not from a Claude cloud session](#why-not-from-a-claude-cloud-session) below.

## Steps

```bash
# 1. Authenticate with GitHub (CK Engineer is pulled from a private GitHub repo)
gh auth login

# 2. Install the CLI
npm i -g claudekit-cli

# 3. Install CK Engineer into this project
cd /path/to/TeaSun
ck init

# ...or install once for every project on the machine
ck init -g
```

Verify afterwards with `ck doctor`.

## Notes for this repository

- `ck init` writes into a `docs/` folder by default. This repo already keeps the label-compliance
  deliverables under `docs/us-label-compliance/`, so pass `--docs-dir <name>` if `ck init` reports a
  conflict.
- `ck init` also creates/updates `CLAUDE.md` and `.claude/settings.json`. Neither exists here yet, so
  the first run has nothing to merge.
- Non-interactive equivalent: `ck init -y --skip-setup` (defaults to kit `engineer`, dir `.`, latest
  version).

## Requirements

| Requirement | Notes |
|---|---|
| Node.js + npm | v22 / v10 verified working with `claudekit-cli` v4.5.2 |
| `gh` CLI, authenticated | `ck` uses it for GitHub access by default; `GITHUB_TOKEN` (classic PAT) or `ck init --use-git --release <tag>` are the alternatives |
| Access to `github.com/claudekit/claudekit-engineer` | The kit content is downloaded from there |

## Why not from a Claude cloud session

`npm i -g claudekit-cli` succeeds, but `ck init` cannot complete:

- GitHub egress in a cloud session is scoped to the repositories attached to that session. Requests to
  `claudekit/claudekit-engineer` return `401`/`403`, and the repo cannot be attached because it belongs
  to a different owner than this session's sources.
- `ck init --use-git` hits the same wall — unauthenticated `git ls-remote` against that repo fails.
- The container is ephemeral anyway, so a globally installed CLI would not survive the session.

Run the steps above locally instead, then commit whatever `ck init` generates.
