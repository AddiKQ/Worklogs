# Worklogs

This repository is a work tracker, not a codebase. It holds tasks for *other* repositories.
One task = one markdown file under `repos/<repo-name>/<status>/`. The folder is the status.

```
repos/<repo-name>/backlog/   not started
repos/<repo-name>/active/    claimed by a worker
repos/<repo-name>/review/    worker says done, orchestrator has not accepted
repos/<repo-name>/blocked/   cannot proceed, reason is in the worklog
repos/<repo-name>/done/      orchestrator accepted
```

## Which role are you?

- **Worker** — your prompt names a repository (`example-repo`) or a task (`example-repo#0004`).
  Read `AGENT.md` and follow it exactly. Touch only `repos/<that-repo>/`.
- **Orchestrator** — your prompt says orchestrate, triage, review, plan, or asks you to create tasks.
  Read `ORCHESTRATOR.md`.
- **Unclear** — run `scripts/wl board`, then ask the user which repo you are working on.

## Rules for everyone

1. Use `scripts/wl` for every status change. Never hand-edit front matter or move task files by hand.
2. One task per commit. Message format: `[<repo>#<id>] <verb>: <summary>` (the script does this for you).
3. `wl` pulls before it changes anything and pushes right after. Do not batch.
4. Never touch another repository's folder.
5. Never delete a task file. Move it to `done/` or `blocked/`.
6. The board is never committed. Run `scripts/wl board` to see it.
7. Set `WL_OWNER` to your session URL (or a short agent name) before you run `wl`, so the worklog says who did what.
