---
name: triage
description: Orchestrator. Process the inbox: merge worker branches, review tasks in review/, decide on blocked tasks, mark done or return. Use when asked to triage, review what came in, or orchestrate.
---

Follow `ORCHESTRATOR.md`.

1. `scripts/wl inbox`.
2. For every unmerged branch listed: `scripts/wl merge <branch>`. Then run `scripts/wl inbox` again.
3. For every task in review/: open the file, compare Result to Acceptance criteria, open the PR or branch in the target repository and verify. Then one of:
   - `scripts/wl move <repo> <id> done -m "accepted: <what you checked>"`
   - `scripts/wl move <repo> <id> backlog -m "returned: <what is missing>"`
4. For every blocked task: decide. Unblock with a `wl log` note and `wl move ... backlog`, or leave it and say why.
5. Finish with `scripts/wl board` and report: accepted, returned, still blocked, and anything that needs the user.

Never mark a task done without opening the work in the target repository.
