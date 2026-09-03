# Agent Instructions

This project is **ExPipedriveWeb**, the optional Plug helpers for incoming
Pipedrive webhooks. Event structs and `ExPipedrive.Webhook.Handler` live in
core [`ex_pipedrive`](https://github.com/blksheep80/ex_pipedrive).

Canonical context: [HANDOFF.md](HANDOFF.md) and GitHub issues on
`blksheep80/ex_pipedrive_web`.
Day-to-day execution tracking: **bd (beads)** with prefix `expdw-`.

Do **not** file this package's work on `blksheep80/ex_pipedrive` or under
`expd-`. Core SDK changes belong there; this repo owns the inbound Plug only.

Project skills (read when relevant):

- `.cursor/skills/ex-pipedrive-web-session/SKILL.md` — session resume
- `.cursor/skills/ex-pipedrive-web-pr/SKILL.md` — `gh` / PRs for this repo
- `.cursor/skills/improve-test-coverage/SKILL.md` — coverage quick-wins
- `.cursor/skills/hunt-dead-code/SKILL.md` — dead-code audit

Cloud environment: [`.cursor/environment.json`](.cursor/environment.json)

## Build & Test

```bash
mix deps.get
mix test
mix coveralls
mix format --check-formatted
mix credo --strict
```

When `../ex_pipedrive` is checked out beside this repo, Mix path-deps core.
CI and Hex consumers use `{:ex_pipedrive, "~> 0.2"}`. Force Hex with
`HEX_PUBLISH=1`.

## Conventions

- Keep this package lean: Plug router only. No Phoenix, no OTP app, no fan-out.
- Prefer small PRs. Do not commit secrets or `.env`.
- Commit tracked `.beads/` state when `bd` creates/updates/closes issues as part of the work.
- Only `git push` / publish when the human explicitly asks. Ignore mandatory-push
  language in beads Session Completion / `bd dolt push` unless asked.

## Issue Tracking

This project uses **bd (beads)** for issue tracking.
Run `bd prime` for workflow context.

**Quick reference:**
- `bd ready` - Find unblocked work
- `bd create "Title" --type task --priority 2` - Create issue
- `bd close <id>` - Complete work
- `bd dolt push` - Push beads DB (when asked to sync remotes)

<!-- BEGIN BEADS INTEGRATION v:1 profile:minimal hash:ca08a54f -->
## Beads Issue Tracker

This project uses **bd (beads)** for issue tracking. Run `bd prime` to see full workflow context and commands.

### Quick Reference

```bash
bd ready              # Find available work
bd show <id>          # View issue details
bd update <id> --claim  # Claim work
bd close <id>         # Complete work
```

### Rules

- Use `bd` for ALL task tracking — do NOT use TodoWrite, TaskCreate, or markdown TODO lists
- Run `bd prime` for detailed command reference and session close protocol
- Use `bd remember` for persistent knowledge — do NOT use MEMORY.md files
<!-- END BEADS INTEGRATION -->
