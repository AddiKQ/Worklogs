# Prompts

Copy-paste prompts for starting Claude sessions. Replace `<...>` placeholders.
Worklogs URL: `https://github.com/AddiKQ/Worklogs`. Its helper script is `scripts/wl` (run `scripts/wl help`).

---

## 0. Universal brief for any coding engine

Paste this into any agent or coding tool (Claude Code, Codex, Cursor, Copilot agent, anything that can run git and shell). It explains the repository and says exactly what to read. Prepend it to any of the prompts below, or use it alone and add your request at the end.

```
# Worklogs: the work tracker you report to

Repository: https://github.com/AddiKQ/Worklogs (branch: main)

Worklogs is not a codebase. It is a git-native issue tracker for other repositories.
One task = one markdown file. The folder it sits in is its status:

  repos/<repo-name>/backlog/   not started
  repos/<repo-name>/active/    claimed by a worker
  repos/<repo-name>/review/    worker says done, orchestrator has not accepted
  repos/<repo-name>/blocked/   cannot proceed, reason is in the worklog
  repos/<repo-name>/done/      orchestrator accepted

Each task file has YAML front matter (id, repo, title, status, priority P0..P3, owner, branch, pr)
and sections: Goal, Context, Acceptance criteria (checkboxes), Result, Worklog.
Every status change is one git commit with the message prefix [<repo>#<id>], so git log is the audit trail.

Two roles exist. A WORKER takes one task, does the work in the target repository, and hands it back.
An ORCHESTRATOR writes tasks, reviews results, merges worker branches, and is the only one who marks done.

## Read these files, in this order, before doing anything
1. CLAUDE.md          which role you are and the rules everyone follows (short)
2. AGENT.md           if you are a worker: the step-by-step protocol
   ORCHESTRATOR.md    if you are an orchestrator
3. repos/<repo>/README.md   how to get, branch, test and PR the target repository
4. the task file itself     once you have claimed or been given one
5. PROMPTS.md         only if you need the ready-made prompts or the onboarding snippet

## The only tool
scripts/wl   (run `scripts/wl help`). It pulls, edits the task file, commits and pushes in one step.
  worker:        next <repo> | claim <repo> <id> | log <repo> <id> "msg" | finish <repo> <id> --branch B --pr URL -m "msg" | move <repo> <id> blocked -m "why"
  orchestrator:  new <repo> "title" -p P1 | save "msg" | inbox | merge <branch> | add-repo <name> <url> | snippet <name>
  anyone:        board [repo] | feed [n] | show <repo> <id>
Set WL_OWNER to your session URL or a short name first, so the worklog records who did what.

## Hard rules
- Never hand-edit front matter or move task files by hand. Use scripts/wl.
- Touch only repos/<your-repo>/. Never delete a task file.
- Workers stop at review/. Only the orchestrator moves a task to done/.
- Work on main in Worklogs. If a push to main is refused, push to a branch named claude/<repo>-<id> and say so; the orchestrator merges it.
- If your session starts in the target repository, clone Worklogs next to it (`git clone https://github.com/AddiKQ/Worklogs ../Worklogs`) and run `../Worklogs/scripts/wl ...`.

## Your assignment
<one of: "Work on <repo>" | "Work on <repo>#<id>" | "Triage worklogs" | "Create tasks for <repo>: ..." | "Onboard this repository to Worklogs as <repo>">
```

---

## A. Session starts in the target repository

Use when you open a session in the code repository itself. The session clones Worklogs next to the code and reports there.

### A1. Work a known task

```
You are a worker for the task <repo>#<id> tracked in https://github.com/AddiKQ/Worklogs.

Setup, before touching code:
1. Clone Worklogs next to this checkout: `git clone https://github.com/AddiKQ/Worklogs ../Worklogs` (use add_repo AddiKQ/Worklogs first if the clone is refused). Stay on its `main` branch.
2. Read `../Worklogs/CLAUDE.md` and `../Worklogs/AGENT.md`. Follow AGENT.md exactly, with one change: the target repository is the one you are already in, so skip the clone step there.
3. `export WL_OWNER="<your session URL>"` and run `../Worklogs/scripts/wl claim <repo> <id>`. It prints the task file. Read it in full.

While working: checkpoint with `../Worklogs/scripts/wl log <repo> <id> "<fact>"` at every meaningful point.

When done: fill the task file's ## Result, tick the acceptance boxes you met, then
`../Worklogs/scripts/wl finish <repo> <id> --branch <your branch here> --pr <PR url if any> -m "<one line>"`.
If Worklogs refuses a push to main, push to a branch named `claude/<repo>-<id>` there and tell me the branch name.

Do not mark the task done. Do not edit any other folder under ../Worklogs/repos/.
```

### A2. Work with no task yet (ad hoc request)

```
Before you start, track this work in https://github.com/AddiKQ/Worklogs:
1. `git clone https://github.com/AddiKQ/Worklogs ../Worklogs` (add_repo AddiKQ/Worklogs first if needed). Read `../Worklogs/CLAUDE.md` and `../Worklogs/AGENT.md`.
2. `export WL_OWNER="<your session URL>"`.
3. If `../Worklogs/repos/<repo>/` does not exist, run `../Worklogs/scripts/wl add-repo <repo> <this repo's GitHub URL>` and fill the README it creates.
4. Run `../Worklogs/scripts/wl new <repo> "<short imperative title>" -p P2`, write Goal / Context / Acceptance criteria from my request below, then `../Worklogs/scripts/wl save "[<repo>#<id>] new: <title>"`.
5. `../Worklogs/scripts/wl claim <repo> <id>`.

Then do the work here, checkpoint with `wl log`, and hand over with `wl finish` as AGENT.md says. Never mark it done.

The request:
<describe the work>
```

### A3. Onboard a repository (one time)

Run this once per repository so every future session there reports to Worklogs on its own.

```
Add Worklogs tracking to this repository.

1. If CLAUDE.md does not exist at the repo root, create it. Otherwise append to it.
2. Add the section below verbatim, replacing <repo> with the name this repository uses in Worklogs (ask me if unsure; default is the GitHub repo name in lower case).
3. Commit on your current branch with message "Track work in Worklogs" and push. Do not open a PR unless I ask.

Section to add:

## Work tracking

All work in this repository is tracked in https://github.com/AddiKQ/Worklogs under `repos/<repo>/`. No untracked work.

Before changing code:
1. `git clone https://github.com/AddiKQ/Worklogs ../Worklogs` if it is not there (add_repo AddiKQ/Worklogs first if the clone is refused). Stay on its main branch.
2. Read `../Worklogs/AGENT.md` and follow it. The target repository is this one.
3. `export WL_OWNER="<your session URL or name>"`.
4. Given a task id: `../Worklogs/scripts/wl claim <repo> <id>`. Given none: `../Worklogs/scripts/wl new <repo> "<title>"`, fill Goal / Context / Acceptance criteria, `wl save`, then claim it.

During work: `../Worklogs/scripts/wl log <repo> <id> "<fact>"` at meaningful points.
When done: fill ## Result in the task file, then `../Worklogs/scripts/wl finish <repo> <id> --branch <branch> --pr <url> -m "<summary>"`.
If a push to Worklogs main is refused, push to `claude/<repo>-<id>` there and report the branch.
Never mark a task done and never touch other folders under `../Worklogs/repos/`.
```

Then, on the Worklogs side, register the repo if it is not there yet:

```
Orchestrate Worklogs. Add the repository <name> at <github url>: run scripts/wl add-repo, fill repos/<name>/README.md (how to get the code, branch rules, test command, PR policy), and save.
```

---

## B. Session starts in Worklogs

Use when you open the session in this repository. `CLAUDE.md` does the routing; the prompt only needs the target.

### B1. Work the next task for a repo

```
Work on <repo>.
```

### B2. Work a specific task

```
Work on <repo>#<id>.
```

### B3. Work a task, with extra instructions

```
Work on <repo>#<id>. Also: <anything the task file does not say>.
```

---

## C. Orchestrator sessions

Start these in Worklogs.

### C1. Triage

```
Triage worklogs.
```

Runs `/triage`: merges worker branches, reviews everything in review/, decides on blocked tasks, reports.

### C2. Create tasks

```
Create tasks for <repo>:
- <one line per task, imperative>
- ...
Priority P1 unless I say otherwise.
```

### C3. Plan a feature into tasks

```
Plan this for <repo> as Worklogs tasks. Read repos/<repo>/README.md first. Split into tasks a single session can finish in one sitting, each with verifiable acceptance criteria, ordered so earlier tasks unblock later ones. Create them with wl new, then show me the board.

<feature description>
```

### C4. Status

```
Show the board.
```

---

## Notes

- `WL_OWNER` is how the worklog says who did what. A session URL is best; a short name is fine.
- `wl` always pulls before it changes a task and pushes right after, so a session never holds stale state.
- Only the orchestrator moves tasks to `done/`. Workers stop at `review/`.
