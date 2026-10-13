# example-repo

Repository: https://github.com/AddiKQ/example-repo

Fill every section. A worker reads only this file and the task file before touching the code.

## Get the code

```bash
# in a Claude cloud session:
add_repo owner=AddiKQ repo=example-repo
git clone https://github.com/AddiKQ/example-repo /home/user/example-repo
```

## Branch rules

- Default branch: `main`
- Work branches: `task/<id>-<slug>`
- Never push to the default branch directly.

## Build and test

```bash
# e.g. npm ci && npm test
```

## PR policy

- Open a PR against `main` when the task is finished. Title: `[example-repo#<id>] <title>`.
- Put the PR URL in the task via `wl finish --pr`.

## Conventions and gotchas

- 
