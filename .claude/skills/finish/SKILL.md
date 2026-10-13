---
name: finish
description: Worker. Hand a finished task back to the orchestrator. Fills the Result section, records branch and PR, moves the task to review/.
---

Arguments: `<repo> <id>`.

1. Open the task file in `repos/<repo>/active/`.
2. Edit `## Result`: what changed, what did not, how to verify. Three to eight lines.
3. Tick every acceptance checkbox you met. Leave unmet ones unticked and say why in Result.
4. Run:

```bash
scripts/wl finish <repo> <id> --branch <branch in target repo> --pr <PR url or omit> -m "<one-line summary>"
```

5. Tell the user: task id, target-repo branch and PR, and the Worklogs branch you pushed to (`git rev-parse --abbrev-ref HEAD`).

If the work cannot be completed, use `scripts/wl move <repo> <id> blocked -m "<why and what would unblock>"` instead.
