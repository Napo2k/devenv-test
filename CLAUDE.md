# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

A [devenv.sh](https://devenv.sh/) configuration that sets up a reproducible Go development environment using Nix. The three key files are:

- `devenv.nix` — environment definition (packages, languages, scripts, tasks, shell hooks, tests)
- `devenv.yaml` — Nix flake inputs (nixpkgs version, custom git-hooks from `github:napo2k/devenv-test`)
- `devenv.lock` — pinned input hashes

## Common commands

```bash
devenv shell        # enter the dev shell (runs enterShell hook on entry)
devenv test         # run the enterTest block
devenv tasks run <name>   # run a named task (e.g. devenv tasks run test:setup)
devenv update       # update devenv.lock to latest input revisions
```

Inside the shell, the custom script `hello` is available (echoes `$GREET`).

## Architecture notes

- **Task ordering**: `devenv:enterShell` depends on `test:setup`, so `go mod init` runs before the shell is fully entered if `go.mod` is missing.
- **Custom git-hooks input**: `devenv.yaml` pulls git-hooks from `github:napo2k/devenv-test` (not the standard devenv git-hooks). Changes to that upstream affect hook behaviour.
- **Go module init**: `test:setup` initialises `go.mod` using `$REPO` as the module name — ensure `REPO` is set in the environment or extend `devenv.nix` to set it via `env.REPO`.
