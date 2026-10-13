# Worklogs

A git-native work tracker for Claude agents working across many repositories.

- One task is one markdown file. The folder it sits in is its status.
- Each target repository gets its own folder under `repos/`. Agents only look at theirs.
- Every status change is one commit with a `[repo#id]` prefix, so `git log` is the audit trail.
- An orchestrator session owns `main`, writes tasks, reviews results, and merges worker branches.

## Layout

```
CLAUDE.md               entry point for any agent: pick a role, read the right doc
AGENT.md                worker protocol
ORCHESTRATOR.md         orchestrator protocol
PROMPTS.md              universal brief for any coding engine, plus worker, orchestrator and onboarding prompts
scripts/wl              the only tool you need (new, next, claim, log, finish, move, board, inbox, merge, feed, add-repo, snippet, save)
templates/              task, repo README and target-repo CLAUDE.md section templates
repos/<name>/README.md  how to work on that repository
repos/<name>/backlog|active|review|blocked|done/
.claude/skills/         slash commands: /claim /finish /log /board /triage /new-task
```

## Quick start

```bash
export WL_OWNER="me"
scripts/wl add-repo my-service https://github.com/org/my-service
scripts/wl new my-service "Add retry to payment client" -p P1
# edit the printed file, then
scripts/wl save "[my-service#0001] new: Add retry to payment client"
```

Then start a Claude session in this repository with a prompt like
`Work on my-service` or `Work on my-service#0001`. The session reads `CLAUDE.md`, claims the task, works in the target repository, and reports back.

Sessions can also start in the target repository and report here. Run `scripts/wl snippet my-service` and add the printed section to that repository's `CLAUDE.md`, or use the onboarding prompt in `PROMPTS.md`.

Start an orchestrator session with `Triage worklogs` or `Review what came in`. All prompts are in `PROMPTS.md`.

## Task lifecycle

```
backlog --claim--> active --finish--> review --accept--> done
                     |                   |
                     +------ blocked <---+        (any state can go back to backlog)
```

## Why files and folders

- `ls repos/my-service/active` answers "what is in flight" with no tooling.
- Moving a file is a rename commit, which git merges cleanly even when many agents work at once.
- There is no shared index file to conflict on. The board is generated on demand.
