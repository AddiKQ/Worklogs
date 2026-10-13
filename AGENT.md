# Worker protocol

You take one task from `repos/<repo>/backlog/`, do the work in the **target** repository, and report back here.
The orchestrator marks tasks done. You never do.

## 0. Identify yourself

```bash
export WL_OWNER="<your session URL, or a short agent name>"
```

## 1. Pick a task

```bash
scripts/wl next <repo>          # top backlog tasks, best first
scripts/wl claim <repo> <id>    # moves it to active/, stamps owner, commits, pushes
```

- If the user gave you a task id, claim that one.
- If `claim` stops with a rebase conflict, another worker took it. Run `git rebase --abort` and claim the next one.

## 2. Read before you act

- The task file, now in `repos/<repo>/active/`. Goal, Context, Acceptance criteria.
- `repos/<repo>/README.md`. Where the code lives, branch rules, test command, PR policy.

## 3. Do the work in the target repository

- Get the repo the way its README says (usually `add_repo` then clone).
- Branch `task/<id>-<slug>` unless the README says otherwise.
- Push. Open a PR there if the README says to.
- Checkpoint here at meaningful points, not only at the end:

```bash
scripts/wl log <repo> <id> "tests green except 2 in auth, investigating"
```

## 4. Report back

1. Edit the task file. Fill `## Result`: what changed, what did not, how to verify. Tick the acceptance boxes you met.
2. Record branch and PR and hand it over:

```bash
scripts/wl finish <repo> <id> --branch task/0004-slug --pr https://github.com/... -m "one-line summary"
```

That moves it to `review/`, commits, pushes.

Blocked instead?

```bash
scripts/wl move <repo> <id> blocked -m "why, and what would unblock it"
```

## 5. Stop

Tell the user the task id, the branch in the target repo, and the branch you pushed to here. The orchestrator takes it from there.

## Where `wl` pushes

`wl` commits and pushes to your **current** branch.

- If you can push to `main`, work on `main`. That is the default and the fastest path.
- If your session is pinned to a branch (for example `claude/...`), stay on it. The orchestrator merges it. Name the branch in your final message.

## Starting from the target repository instead

If your session began in the code repository, not here: clone Worklogs next to it (`git clone https://github.com/AddiKQ/Worklogs ../Worklogs`, `add_repo AddiKQ/Worklogs` first if refused), stay on its `main`, and run every command above as `../Worklogs/scripts/wl ...`. Skip the clone step in section 3; you are already there. Ready-made prompts are in `PROMPTS.md`.

## Conflicts

Only the task file changes, so a conflict means two agents touched the same task.

1. `git rebase --abort`
2. If you were claiming: pick another task.
3. If you were finishing: `git pull --rebase`, re-apply your Result section on top of theirs, run `wl finish` again.
