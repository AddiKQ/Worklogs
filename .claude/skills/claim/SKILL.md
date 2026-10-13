---
name: claim
description: Worker. Claim the next backlog task (or a given id) for a repository in Worklogs and start working it. Use when the prompt says "work on <repo>" or "work on <repo>#<id>".
---

Arguments: `<repo> [id]`.

1. If `WL_OWNER` is unset, export it as your session URL or a short agent name.
2. No id given: run `scripts/wl next <repo>` and take the first row.
3. Run `scripts/wl claim <repo> <id>`. It prints the task file path.
   - Rebase conflict: `git rebase --abort`, take the next row, claim again.
4. Read the task file and `repos/<repo>/README.md` in full.
5. Follow `AGENT.md` sections 3 to 5: work in the target repository, checkpoint with `/log`, hand over with `/finish`.

Touch nothing outside `repos/<repo>/` in this repository.
