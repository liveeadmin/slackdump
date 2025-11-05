# v.checkpoint — Clean, Buildable, Documented State
**Purpose**: Freeze a milestone with validated code and up-to-date memory.
**Version**: 2.0.0 | **Last Updated**: 2025-11-04


## Steps

### 1. Git Status Check
```bash
git status --short
git diff --stat
```
- If uncommitted changes exist, analyze what's ready to commit
- If untracked files exist, list them for user decision

### 2. File Size & Quality Checks
- Run `v.shrink` (file size guardrails)
- Run `v.testsync` (tests compile with current APIs)
- Build + run unit tests; categorize failures (test vs behavioral)

### 3. Memory Sync
- Run `v.memorize` (update progress/changelog and rotate archives)
- If `_ai/memory/archive/index.md` exists, refresh it from current archive files
- Ensure `progress.md` has latest findings and decisions

### 4. Git Commit
Determine commit type from current task in `plan.md`:
```bash
# Read active task ID and description from progress.md or resume.md
# Generate appropriate commit message:

# For completed task:
git add -A
git commit -m "feat(T-XXX): <task-title>"

# For checkpoint mid-work:
git add -A
git commit -m "checkpoint: T-XXX in progress - <brief-state>"

# For phase completion:
git add -A
git commit -m "checkpoint: Phase PH-XX complete - <summary>"
```

### 5. Verification
- Confirm commit created: `git log -1 --oneline`
- Verify working directory clean: `git status`
- Output commit hash and message to user

## Result
- All files respect size policy
- Tests compile; build passes
- Memory synced; archive index refreshed
- Changes committed with proper message
- Clean working directory

## Options
- `--no-commit` — Run checks but skip git commit
- `--branch <name>` — Create feature branch before committing
- `--amend` — Amend previous commit instead of creating new one

## Notes
- If legacy or divergent memory detected, run `v.initmemory` before committing
- For experimental work, suggest creating feature branch first
- Always show git diff summary before committing
