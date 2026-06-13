# AGENTS.md

This file provides guidance to AI agents working in this repository.
`api_demo_server` is a small FastAPI server (PostgreSQL + Prometheus metrics)
used as the reference app in the Ops Kubernetes charm tutorial. The code lives
in `src/api_demo_server/`. Dependencies and tools are managed with
[`uv`](https://docs.astral.sh/uv/).

## Lint, format, test

Tasks run through `make` (a thin wrapper over `uv run`):

```bash
make format       # uv run ruff format; ruff check --fix
make lint         # ruff check; ruff format --diff; ty check
make integration  # docker compose up + curl smoke checks (needs Docker)
```

There are no unit tests — `make integration` (`.scripts/integration-test.sh`)
brings the stack up with `docker compose`, exercises the create/add/list
endpoints over HTTP, and tears it down. It needs a working Docker daemon.

## Conventions

- **Type checking uses [`ty`](https://github.com/astral-sh/ty)**, not mypy or
  pyright; it's pinned in the `dev` dependency group and run via `make lint`.
- **Linting/formatting is `ruff`** (line length 99).
- **Runtime deps are exact-pinned** in `pyproject.toml` (e.g. `fastapi==…`);
  keep them pinned and let Dependabot bump them.
- **Commits / PR titles:** [Conventional Commits](https://www.conventionalcommits.org/)
  (`chore`, `ci`, `docs`, `feat`, `fix`, `perf`, `refactor`, `revert`, `test`),
  no scopes.
- The published artifact is a multi-arch Docker image built from `Dockerfile`.
