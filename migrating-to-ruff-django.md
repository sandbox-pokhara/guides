# Migrating to `ruff` from `black` + `isort`

1. Remove `venv` (if exists), and create new `.venv`.
1. In `pyproject.toml`, change pyright `venv` path.

   ```toml
   [tool.pyright]
   venvPath = "."
   venv = ".venv"
   ```

1. Remove `black` and `isort` from `pyproject.toml` and add these

   ```toml
   [tool.ruff]
   src = ["./backend"]
   exclude = ["./backend/**/migrations/"]

   [tool.ruff.lint]
   select = ["I"]
   ```

1. Rename `venv` to `.venv` in Dockerfile.

1. In `pre-commit.yml`, rename `venv` to `.venv`.

1. Remove `black` and `isort` from `.pre-commit-config.yaml` and add these:

   ```yml
   - repo: https://github.com/astral-sh/ruff-pre-commit
     rev: v0.12.2
     hooks:
       - id: ruff-check
         args: [--fix]
       - id: ruff-format
   ```

1. Run `pre-commit autoupdate` to make sure that all repos in pre-commit is up-to-date.
1. Run `pre-commit run --all-files`, `ruff` will fail the first time and pass the second time.
