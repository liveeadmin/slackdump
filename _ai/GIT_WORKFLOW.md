# Git Workflow Guide
**Version**: 1.0.0 | **Last Updated**: 2025-11-04


## Philosophy
**Commit early, commit often. Never lose working code.**

This framework integrates git workflow directly into the development process to ensure safety and traceability.

## Automatic Safety Checks

### Before Every Implementation (`v.do`, `v.implement`)
The agent ALWAYS checks git status before starting work:

```bash
✅ Clean directory → Proceed
⚠️  ≤10 lines changed → Proceed with caution
🛑 >10 lines uncommitted → STOP and suggest v.checkpoint
```

### Checkpoint Triggers

Automatic reminders to run `v.checkpoint`:
- ✅ After completing 3 steps in a task
- ✅ After completing a full task
- ✅ After large refactoring operations
- ✅ Before running `v.shrink`

## Commit Message Convention

Following **Conventional Commits** format with Task IDs:

```bash
# Feature implementation
feat(T-010): add user authentication

# Bug fix
fix(T-023): resolve login redirect issue

# Refactoring
refactor(T-045): split UserService into modules

# Documentation
docs(T-012): update API documentation

# Tests
test(T-034): add integration tests for payment

# Checkpoints (mid-work)
checkpoint: before T-015 - about to refactor database
checkpoint: T-020 in progress - API endpoints working
checkpoint: Phase PH-02 complete - MVP features done
```

## Branch Strategy

### When to Use Branches

**Create a feature branch** for:
- Experimental features (uncertain if they'll work)
- Risky refactoring (might need to revert)
- Large changes spanning multiple tasks
- When explicitly requested by user

**Work directly on main** for:
- Small, well-defined tasks
- Bug fixes with clear solution
- Documentation updates
- When tests pass and changes are verified

### Branch Naming Convention

```bash
feature/T-XXX-short-description
fix/T-XXX-bug-description
refactor/T-XXX-what-changed
```

### Branch Workflow

```bash
# 1. Create and switch to branch
git checkout -b feature/T-015-dark-mode

# 2. Work and commit normally
# ... implement step by step ...
git commit -m "feat(T-015): add dark mode toggle"

# 3. When satisfied and tests pass
git checkout main
git merge feature/T-015-dark-mode --no-ff
git branch -d feature/T-015-dark-mode

# 4. Or if needs more work
git checkout main  # switch back
# Keep branch for later: git checkout feature/T-015-dark-mode
```

## Recovery Strategies

### Undo Last Commit (Not Pushed)
```bash
# Keep changes in working directory
git reset --soft HEAD~1

# Discard changes completely
git reset --hard HEAD~1
```

### Discard Uncommitted Changes
```bash
# Single file
git restore <file>

# All files
git restore .

# Save for later without committing
git stash
git stash list
git stash pop  # restore later
```

### Rollback After Bad Implementation
```bash
# 1. Check what broke
git status
git diff

# 2. View recent commits
git log --oneline -5

# 3. Revert to previous commit
git reset --hard <commit-hash>

# 4. Document in progress.md
# 5. Update plan.md with new approach
```

## Integration with Memory Files

### progress.md
After each step, document:
```markdown
### [T-015] Dark Mode Feature
**TODO (pre-implementation):**
- [ ] S-0151 — Add theme context provider
- [ ] S-0152 — Implement toggle component

**Findings (post-implementation):**
- Summary: Created ThemeContext with localStorage persistence
- Decisions: Used CSS variables for instant switching
- Evidence: commit abc123, all tests pass
**Git**: feat(T-015): add dark mode foundation
```

### Checkpoint Command
`v.checkpoint` now:
1. ✅ Runs file size checks (v.shrink)
2. ✅ Validates tests compile (v.testsync)
3. ✅ Syncs memory (v.memorize)
4. ✅ **Creates git commit** with proper message
5. ✅ Verifies clean working directory

## Best Practices

### ✅ DO
- Commit after every completed task
- Use descriptive commit messages with Task IDs
- Check git status before starting new work
- Create branches for experimental features
- Document rollbacks in progress.md

### ❌ DON'T
- Leave >10 lines uncommitted when starting new work
- Skip checkpoints after major implementations
- Commit secrets or credentials
- Force push to shared branches
- Rewrite published history

## Quick Reference

```bash
# Check status
git status --short
git diff --stat

# Commit current work
v.checkpoint

# Create feature branch
git checkout -b feature/T-XXX-name

# View history
git log --oneline -10
git log --graph --oneline -20

# Undo changes
git restore <file>           # discard file changes
git reset --soft HEAD~1      # undo last commit, keep changes
git reset --hard HEAD~1      # undo last commit, discard changes

# Branch management
git branch -a                # list all branches
git merge feature/xxx        # merge branch
git branch -d feature/xxx    # delete merged branch
```

## Troubleshooting

### "I forgot to checkpoint and made too many changes"
```bash
# Review what changed
git diff --stat
git diff

# If changes are good, commit now
git add -A
git commit -m "feat(T-XXX): describe what was done"

# Update progress.md to document
```

### "I need to switch tasks but have uncommitted work"
```bash
# Option 1: Commit current state
git add -A
git commit -m "checkpoint: T-XXX partial - describe state"

# Option 2: Stash for later
git stash push -m "T-XXX in progress"
# ... work on other task ...
git stash pop  # resume later
```

### "Tests are failing after implementation"
```bash
# Don't commit broken code
# Either fix tests, or rollback and adjust approach

# Rollback option:
git restore .
# Update plan.md with new approach
# Try again with v.do
```

---

**Remember**: The framework enforces git safety checks automatically. Trust the process and checkpoint frequently!
