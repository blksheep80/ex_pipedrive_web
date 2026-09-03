---
name: ex-pipedrive-web-session
description: >-
  Resume ExPipedriveWeb work correctly: read HANDOFF, check beads (expdw-),
  and keep GitHub issues on this repo. Use at session start, when picking up
  the webhook Plug package, or when the user asks what to work on next.
---

# ExPipedriveWeb session resume

## Do first

1. Read `HANDOFF.md` (locked decisions + current state).
2. Run `bd ready` (and `bd list --status=in_progress` if useful).
3. Confirm `gh repo set-default --view` → `blksheep80/ex_pipedrive_web`. If not:

```bash
gh repo set-default origin
```

4. Prefer GitHub issues on **this** repo for roadmap; create/claim a **bead** (`expdw-`) when starting concrete work.

## Quality gate (when code changes)

```bash
mix test
mix format --check-formatted
mix credo --strict
```

## Tracking split

| Layer | Use for |
|---|---|
| `HANDOFF.md` | Where we are / locked decisions |
| GitHub issues (`ex_pipedrive_web`) | Product backlog, acceptance criteria |
| beads (`expdw-`) | Work in flight; close with reason |

Do not use TodoWrite / markdown TODO lists for task tracking in this repo.
Do not file this package's work on core (`expd-` / `blksheep80/ex_pipedrive`).
