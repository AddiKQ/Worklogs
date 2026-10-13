---
name: new-task
description: Orchestrator. Create a well-formed backlog task for a repository from a short description. Use when asked to add, file, or create a task or issue.
---

Arguments: `<repo> "<title>" [P0..P3]`. If the repo folder does not exist, run `scripts/wl add-repo <name> <url>` first and ask the user for the URL if unknown.

1. `scripts/wl new <repo> "<title>" -p <priority>` prints the new file.
2. Edit it so a worker with no other context can succeed:
   - Goal: the outcome and why, one paragraph.
   - Context: file paths, links, decisions already made, things not to do.
   - Acceptance criteria: three to six checkboxes, each verifiable.
3. `scripts/wl save "[<repo>#<id>] new: <title>"`.

Default priority is P2. Ask only if the description is too thin to write acceptance criteria.
