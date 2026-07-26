# v.debug — Structured Bug Investigation and Resolution

**Version**: 1.0.0 | **Last Updated**: 2025-11-05

**Purpose**: Plan and track non-trivial bug investigations with structured debugging tasks. Converts interactive debugging into a documented, reproducible process.

---

## Usage

```bash
v.debug <subcommand> [args]
```

**Subcommands**:
- `new <description>` — Create new debug investigation
- `list` — List all active debug sessions
- `switch <id>` — Switch to different debug session
- `pause [id]` — Pause current debug session
- `resume <id>` — Resume paused debug session
- `resolve [id]` — Mark bug as resolved
- `status` — Show current active debug session
- `delete <id>` — Delete debug session (with confirmation)

**Examples**:
```bash
v.debug new "Photos fail to load after album deletion"
v.debug list
v.debug switch D-002
v.debug pause
v.debug resume D-001
v.debug resolve D-003
v.debug status
```

---

## When to Use v.debug

**Use `v.debug`** for:
- ✅ Complex bugs requiring multiple investigation steps
- ✅ Intermittent or hard-to-reproduce issues
- ✅ Bugs affecting multiple components
- ✅ Performance issues requiring profiling
- ✅ Security vulnerabilities needing careful analysis
- ✅ Regressions requiring git bisect or historical analysis

**Don't use `v.debug`** for:
- ❌ Simple typos or syntax errors (just fix them)
- ❌ Known solutions (create task in plan.md)
- ❌ Feature requests (use `v.feature` instead)
- ❌ Trivial bugs solvable in one step

---

## Debug Session Directory Structure

```
_ai/debug/
├── active.md              # Current active debug session
├── D-001-photo-load-fail/
│   ├── description.md     # Bug description and reproduction
│   ├── hypothesis.md      # Current theories and tests
│   ├── investigation.md   # Step-by-step investigation log
│   ├── solution.md        # Proposed fix and validation
│   └── progress.md        # Debug session progress
├── D-002-memory-leak/
│   ├── description.md
│   ├── hypothesis.md
│   ├── investigation.md
│   └── progress.md
└── resolved/              # Archived resolved bugs
    └── D-001-photo-load-fail/ (moved here when resolved)
```

---

## Subcommands

### 1. Create New Debug Session (`v.debug new`)

```bash
v.debug new "Photos fail to load after album deletion"

# Creates:
# _ai/debug/D-003-photos-fail-load/
# _ai/debug/D-003-photos-fail-load/description.md (template)
# _ai/debug/D-003-photos-fail-load/hypothesis.md
# _ai/debug/D-003-photos-fail-load/investigation.md
# _ai/debug/D-003-photos-fail-load/progress.md
# Updates active.md to D-003
```

**Output**:
```
Created: _ai/debug/D-003-photos-fail-load/
Active debug session: D-003

Next steps:
1. Document bug reproduction in description.md
2. Form hypotheses in hypothesis.md
3. Run investigation steps
4. Document findings in investigation.md
5. Propose solution in solution.md

Use: v.debug status (show details)
     v.next (see next investigation step)
```

### 2. Debug Session Template (description.md)

When creating new debug session, generates:

```markdown
# Bug D-003: Photos fail to load after album deletion

## Description

**Summary**: [One-line bug description]

**Impact**: 
- Severity: [Critical/High/Medium/Low]
- Users affected: [Number or percentage]
- Components: [List affected components]

## Reproduction Steps

1. [Step 1]
2. [Step 2]
3. [Step 3]

**Expected Behavior**: [What should happen]

**Actual Behavior**: [What actually happens]

## Environment

- Browser: [Chrome 120, Firefox 118, etc.]
- OS: [macOS 14, Windows 11, etc.]
- Version: [App version or commit SHA]
- Data: [Any special data conditions]

## Error Messages

```
[Paste error messages, stack traces, console output]
```

## Related Issues

- Similar to: [Reference to similar bugs]
- Related commits: [Recent changes that might have caused this]
- Related features: [Features that interact with this bug]
```

### 3. Investigation Template (hypothesis.md)

```markdown
# Investigation D-003: Hypotheses

## Current Hypotheses

### Hypothesis 1: Stale photo references after deletion
**Theory**: Album deletion doesn't clear photo references
**Test**: Check if photo IDs persist in state after album delete
**Status**: Testing

### Hypothesis 2: Race condition in async deletion
**Theory**: UI updates before database cleanup completes
**Test**: Add logging to track async operation timing
**Status**: Not tested

### Hypothesis 3: Cache not invalidated
**Theory**: Photo list cached and not refreshed after deletion
**Test**: Force cache clear and retry
**Status**: Disproved (2025-11-05)

## Tested and Eliminated

- ~~Photo validation logic~~ — Works correctly in isolation
- ~~API endpoint~~ — Returns correct data

## Evidence Log

### 2025-11-05 10:30 — Test: Photo ID persistence
```javascript
// After album delete
console.log(state.photos) // Still contains deleted album's photos
```
**Result**: Hypothesis 1 appears correct

### 2025-11-05 10:45 — Test: Async timing
```javascript
// Added timing logs
albumService.delete() // Takes 250ms
photoList.refresh()   // Fires after only 50ms
```
**Result**: Confirms race condition (Hypothesis 2)
```

### 4. Investigation Log Template (investigation.md)

```markdown
# Investigation D-003: Step-by-Step Log

## Investigation Plan

- [ ] Step 1: Reproduce bug consistently
- [ ] Step 2: Identify affected code paths
- [ ] Step 3: Test hypotheses
- [ ] Step 4: Narrow down root cause
- [ ] Step 5: Propose fix
- [ ] Step 6: Validate fix
- [ ] Step 7: Test edge cases

## Session 2025-11-05 10:00-11:30

### Step 1: Reproduce bug ✅
- Created test album "TestAlbum"
- Added 5 photos
- Deleted album via UI
- Observed: Photos still appear in "All Photos" view
- Bug reproduced consistently

### Step 2: Code path analysis ✅
Files examined:
- `src/services/album-service.js:deleteAlbum()`
- `src/components/photo-list.js:refreshPhotos()`
- `src/state/photo-store.js:removePhotosInAlbum()`

Found issue: `removePhotosInAlbum()` is never called!

### Step 3: Root cause identified ✅
**Root cause**: `albumService.deleteAlbum()` doesn't trigger photo cleanup
**Location**: `src/services/album-service.js:45`
**Missing**: Call to `photoStore.removePhotosInAlbum(albumId)`

## Next Steps

- [ ] Implement fix in album-service.js
- [ ] Add unit test for photo cleanup
- [ ] Test with multiple albums
- [ ] Verify no side effects
```

### 5. Solution Template (solution.md)

Created when fix is identified:

```markdown
# Solution D-003: Photo cleanup after album deletion

## Root Cause

**Problem**: `albumService.deleteAlbum()` doesn't trigger photo removal from store

**Location**: `src/services/album-service.js:45`

**Code**:
```javascript
async deleteAlbum(albumId) {
  await db.albums.delete(albumId);
  // Missing: photo cleanup!
  eventBus.emit('album-deleted', albumId);
}
```

## Proposed Fix

**Approach**: Add photo cleanup step before emitting event

**Changed files**:
- `src/services/album-service.js` — Add cleanup call
- `tests/album-service.test.js` — Add test for photo cleanup

**Implementation**:
```javascript
async deleteAlbum(albumId) {
  // Get photos before deletion
  const photos = await db.photos.where({ albumId }).toArray();
  
  // Delete album
  await db.albums.delete(albumId);
  
  // Clean up photos
  await photoStore.removePhotosInAlbum(albumId);
  
  // Emit event
  eventBus.emit('album-deleted', { albumId, photoIds: photos.map(p => p.id) });
}
```

## Validation Plan

- [ ] Unit test: Verify photos removed after album delete
- [ ] Integration test: Delete album in UI, check photo list
- [ ] Edge case: Delete album with no photos
- [ ] Edge case: Delete album with shared photos
- [ ] Performance: Test with large album (1000+ photos)

## Risk Assessment

**Risk Level**: Low
- Small, localized change
- Existing photo cleanup logic already tested
- No breaking API changes

**Rollback Plan**: Revert commit if issues appear

## Related Tasks

After fix validated:
- [ ] T-XXX: Add similar cleanup to folder deletion
- [ ] T-XXX: Audit other delete operations for cleanup gaps
- [ ] T-XXX: Add cleanup verification to test suite
```

### 6. List Debug Sessions (`v.debug list`)

```bash
v.debug list

# Active Debug Sessions

| ID    | Description                    | Status      | Priority | Age    |
|-------|--------------------------------|-------------|----------|--------|
| D-001 | Photo load failure             | Resolved    | High     | 3 days |
| D-002 | Memory leak in album view      | In Progress | Critical | 1 day  |
| D-003 | Photos fail after delete       | In Progress | High     | 2 hrs  |
| D-004 | Slow search with large dataset | Paused      | Medium   | 1 week |

Active: D-003 (Photos fail after delete)

Commands:
  v.debug switch D-002    # Switch to memory leak investigation
  v.debug status          # Show D-003 details
```

### 7. Switch Debug Sessions (`v.debug switch`)

```bash
v.debug switch D-002

# Switching from D-003 to D-002...
# 
# Pausing: D-003 (Photos fail after delete)
# - Current hypothesis: Race condition in async deletion
# - Investigation: 60% complete
# - Solution: Not yet proposed
#
# Activating: D-002 (Memory leak in album view)
# - Status: In Progress (75% complete)
# - Priority: Critical
# - Last step: Profiling heap snapshots
#
# Updated: _ai/debug/active.md
#
# Files available:
#   description.md — Bug reproduction steps
#   hypothesis.md  — Testing race condition theory
#   investigation.md — Profiler results documented
#
# Next: Review hypothesis.md for current theories
```

### 8. Debug Status (`v.debug status`)

```bash
v.debug status

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🐛 Active Debug Session: D-003
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Description: Photos fail to load after album deletion
Priority: High
Status: In Progress
Started: 2025-11-05 10:00
Time spent: 2h 15m

Investigation Progress: 60%
- ✅ Bug reproduced
- ✅ Code path identified
- ✅ Root cause found
- 🔄 Implementing fix
- ⏳ Validation pending

Current Hypothesis:
  Race condition in async deletion
  Status: Confirmed ✅

Root Cause:
  Missing photo cleanup in album-service.js

Next Step:
  Implement fix and write test

Location: _ai/debug/D-003-photos-fail-load/

Files:
  📄 description.md (bug reproduction)
  🔬 hypothesis.md (3 theories, 1 confirmed)
  📋 investigation.md (step-by-step log)
  💡 solution.md (fix proposed, not implemented)
```

### 9. Resolve Bug (`v.debug resolve`)

```bash
v.debug resolve D-003

# Validating resolution...
#
# Checks:
# ✅ Solution implemented
# ✅ Tests pass
# ✅ Fix validated manually
# ✅ No side effects observed
# ✅ Documentation updated
#
# Archiving:
# - Moving D-003/ → _ai/debug/resolved/D-003/
# - Recording resolution in CHANGELOG.md
# - Clearing active.md
#
# Bug D-003 (Photos fail after delete) resolved!
#
# Summary:
#   Root cause: Missing photo cleanup in deleteAlbum()
#   Solution: Added photoStore.removePhotosInAlbum() call
#   Files changed: album-service.js, album-service.test.js
#   Time spent: 2h 30m
#
# Create related tasks? (y/n): y
# > Task created: T-XXX "Audit other delete operations"
```

---

## Integration with Workflow

### Starting a Debug Session

```bash
# User reports bug or you discover one
v.debug new "Search returns wrong results for tags with spaces"

# Document reproduction
# Edit _ai/debug/D-004-search-wrong-results/description.md
# Add steps, expected vs actual behavior

# Form hypotheses
# Edit _ai/debug/D-004-search-wrong-results/hypothesis.md
# List possible causes

# Run investigation using v.do
v.next  # Shows next investigation step
v.do    # Executes investigation step, logs findings
```

### During Investigation

Debug sessions integrate with normal workflow:
- `v.next` reads from `investigation.md` for next step
- `v.do` executes step and updates `investigation.md` and `progress.md`
- `v.checkpoint` commits findings incrementally
- `v.resume` continues interrupted investigation

### Finishing Debug Session

```bash
# After fix implemented and validated
v.debug resolve D-004

# Optionally create follow-up tasks
v.debug resolve D-004 --with-tasks
# Creates tasks for:
# - Similar bugs to check
# - Missing tests to add
# - Documentation to update
```

---

## Debug Session Progress Tracking

In `progress.md` within debug session:

```markdown
# Debug Session D-003: Progress

## Timeline

### 2025-11-05 10:00 — Session Start
- Created debug session
- Documented reproduction steps
- Priority: High (affects all users)

### 2025-11-05 10:30 — Hypothesis Formation
- Identified 3 possible causes
- Prioritized: Race condition most likely

### 2025-11-05 11:00 — Investigation
- Reproduced bug consistently
- Added logging to track timing
- Confirmed race condition hypothesis

### 2025-11-05 11:30 — Root Cause Found
- Missing cleanup call in album-service.js
- Documented in solution.md

### 2025-11-05 12:00 — Fix Implementation
- Added cleanup logic
- Wrote unit tests
- Manual validation successful

## Findings

**Root Cause**: Missing `photoStore.removePhotosInAlbum()` call
**Impact**: All users deleting albums
**Fix**: 5-line change + test
**Validation**: All tests pass, manual test successful

## Lessons Learned

- Always test cleanup paths in delete operations
- Async operations need proper ordering
- Unit tests should verify side effects, not just main functionality

## Related Work

- Similar issue in folder deletion (T-XXX created)
- Need to audit all delete operations (T-XXX created)
```

---

## Integration with v.checkpoint

```bash
# Checkpoint after major discovery
v.checkpoint "debug: found root cause for photo load failure"

# Checkpoint before trying risky fix
v.checkpoint "debug: before implementing cleanup refactor"

# Checkpoint after successful fix
v.checkpoint "fix(D-003): resolve photo cleanup race condition"
```

---

## Debug Command Options

### Advanced Usage

```bash
# Create with priority
v.debug new "Critical security issue" --priority critical

# Create and link to existing task
v.debug new "Regression in T-045" --task T-045

# Resolve with automatic task generation
v.debug resolve D-003 --create-tasks

# Export debug session report
v.debug export D-003 --format markdown > debug-report.md
```

---

## Comparison with Other Commands

| Command | Use Case | Structure |
|---------|----------|-----------|
| `v.feature` | New feature development | spec → plan → tasks |
| `v.debug` | Bug investigation | description → hypothesis → investigation → solution |
| `v.do` | Execute planned task | Single step from plan.md |
| Interactive debug | Simple bugs | No structure, immediate fix |

---

## Benefits of Structured Debugging

1. **Reproducibility**: Clear reproduction steps documented
2. **Knowledge retention**: Investigation findings preserved
3. **Team collaboration**: Others can review or continue investigation
4. **Pattern recognition**: Similar bugs easier to spot
5. **Quality improvement**: Post-mortems inform testing strategy
6. **Time tracking**: Accurate time spent on debugging
7. **Hypothesis testing**: Scientific approach to debugging

---

## Result

- Complex bugs tracked systematically
- Investigation steps documented and repeatable
- Findings preserved for future reference
- Root causes clearly identified
- Solutions validated before implementation
- Follow-up tasks generated automatically

---

## Notes

- Use `v.debug` for bugs that take >30 minutes to investigate
- Simple bugs should be fixed directly without overhead
- Debug sessions can be paused and resumed like features
- Multiple debug sessions can be active simultaneously
- Resolved sessions archived for historical reference
- Integration with git blame and bisect documented in investigation.md
