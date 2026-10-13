# Orchestrator protocol

You own `main`. You write tasks, review what workers report, and mark them done. Workers never mark done.

## Inbox, every time you start

```bash
scripts/wl inbox
```

Shows three things:

1. Tasks in `review/`, waiting for your verdict.
2. Tasks in `blocked/`, waiting for a decision.
3. Remote branches with task changes not yet merged into `main`. These are worker sessions that could not push to `main`.

## Merge worker branches first

```bash
scripts/wl merge <branch>       # merge --no-ff into main, push
```

Each task is its own file, so these merge clean. A conflict means two workers hit one task: keep the later `## Result`, note both in the worklog, continue.

## Review a task in `review/`

1. Open the task file. Compare `## Result` against `## Acceptance criteria`.
2. Open the PR or branch in the target repo. Verify, do not trust the summary.
3. Decide:

```bash
scripts/wl move <repo> <id> done    -m "accepted: <what you checked>"
scripts/wl move <repo> <id> backlog -m "returned: <what is missing>"
scripts/wl move <repo> <id> active  -m "returned to <owner>: <what is missing>"
scripts/wl move <repo> <id> blocked -m "<why>"
```

## Create tasks

```bash
scripts/wl new <repo> "Short imperative title" -p P1    # prints the new file path
```

Edit the file: Goal, Context, Acceptance criteria as checkboxes. A worker gets only this file and the repo README, so make it self-contained: links, file paths, decisions already made, what not to do.

```bash
scripts/wl save "[<repo>#<id>] new: Short imperative title"
```

## Add a repository

```bash
scripts/wl add-repo <name> <github-url>
```

Then fill `repos/<name>/README.md`: how to get the code, branch rules, test command, PR policy. Save with `wl save`.

## See everything

```bash
scripts/wl board [repo]     # every task by status
scripts/wl feed [n]         # last n commits touching repos/, newest first
```

`feed` is the audit trail. Every claim, log, finish and move is one commit with a `[repo#id]` prefix.

## Priorities

| Priority | Meaning |
|---|---|
| P0 | drop everything |
| P1 | this week |
| P2 | normal |
| P3 | when idle |

Workers take the lowest number first, then the lowest id.
