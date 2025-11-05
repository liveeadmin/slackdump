# techenv.md — Environment & Commands

## Toolchain
- Language/Runtime: <version>
- Package Manager: <name + version>
- Formatter/Linter: <tools>
- Test Runner: <tool>

## Commands
- Setup: <cmd>
- Build: <cmd>
- Test (unit): <cmd>
- Test (integration): <cmd>
- Lint/Format: <cmd>
- Size Check: ./check-code-file-sizes.sh --root . [--fix]
- Run: <cmd>

## Versions & IDs
- Bump files: <list of files carrying versions>
- Release process: <short steps>

## Policies
- File hard cap: 600 lines (refactor if exceeded)
- No secrets in repo; use .env.example and env vars
- Logs redact sensitive data

## CI Notes
- Required jobs: lint, unit, size-check
- Optional jobs: integration/e2e (off by default)
