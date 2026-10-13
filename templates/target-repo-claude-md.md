## Work tracking

All work in this repository is tracked in https://github.com/AddiKQ/Worklogs under `repos/{{NAME}}/`. No untracked work.

Before changing code:
1. `git clone https://github.com/AddiKQ/Worklogs ../Worklogs` if it is not there (add_repo AddiKQ/Worklogs first if the clone is refused). Stay on its main branch.
2. Read `../Worklogs/AGENT.md` and follow it. The target repository is this one.
3. `export WL_OWNER="<your session URL or name>"`.
4. Given a task id: `../Worklogs/scripts/wl claim {{NAME}} <id>`. Given none: `../Worklogs/scripts/wl new {{NAME}} "<title>"`, fill Goal / Context / Acceptance criteria, `wl save`, then claim it.

During work: `../Worklogs/scripts/wl log {{NAME}} <id> "<fact>"` at meaningful points.
When done: fill ## Result in the task file, then `../Worklogs/scripts/wl finish {{NAME}} <id> --branch <branch> --pr <url> -m "<summary>"`.
If a push to Worklogs main is refused, push to `claude/{{NAME}}-<id>` there and report the branch.
Never mark a task done and never touch other folders under `../Worklogs/repos/`.
