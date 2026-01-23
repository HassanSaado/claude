# Lint

Run all code quality checks for the current project: linting, formatting, and type checking.

## Detection Rules

1. **Node.js projects** (package.json exists):
   - Run `lint` script if it exists
   - Run `format:check` or `format-check` script if it exists (Prettier)
   - Run `typecheck` script if it exists (TypeScript)
   - Use pnpm if pnpm-lock.yaml exists
   - Use yarn if yarn.lock exists
   - Use npm otherwise

2. **Python projects**:
   - Linting: ruff check, flake8, or pylint
   - Formatting: ruff format --check or black --check
   - Type checking: mypy or pyright if configured

3. **Go projects** (go.mod exists):
   - Run `golangci-lint run` if available, otherwise `go vet ./...`
   - Run `go fmt` check

4. **Rust projects** (Cargo.toml exists):
   - Run `cargo clippy`
   - Run `cargo fmt --check`

## Instructions

1. Detect the project type based on config files in the current working directory
2. Check package.json for available scripts (lint, format:check, typecheck)
3. Run ALL available code quality checks (not just lint)
4. Report any errors found from any check
5. If `--fix` is passed as an argument, run fix commands instead:
   - `lint:fix` instead of `lint`
   - `format` instead of `format:check`

$ARGUMENTS
