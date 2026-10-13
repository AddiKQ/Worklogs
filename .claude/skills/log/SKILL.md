---
name: log
description: Worker. Append a dated checkpoint line to a task's worklog and push it. Use at meaningful points during work, not only at the end.
---

Arguments: `<repo> <id> "<message>"`.

Run `scripts/wl log <repo> <id> "<message>"`.

Good messages state a fact the orchestrator can act on: "tests green", "blocked on missing API key, asked user", "scope smaller than described, skipping X because Y". One line.
