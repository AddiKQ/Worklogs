# {{NAME}}

Repository: {{URL}}

Fill every section. A worker reads only this file and the task file before touching the code.

## Get the code

```bash
# in a Claude cloud session:
add_repo owner={{OWNER}} repo={{NAME}}
git clone {{URL}} /home/user/{{NAME}}
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

- Open a PR against `main` when the task is finished. Title: `[{{NAME}}#<id>] <title>`.
- Put the PR URL in the task via `wl finish --pr`.

## Conventions and gotchas

- 
