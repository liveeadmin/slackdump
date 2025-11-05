# v.doall — Fast Vibe Coding Execution

**Version**: 1.0.0 | **Last Updated**: 2025-11-05

**Purpose**: Execute all tasks automatically without stopping for confirmation. Autonomous problem-solving with TDD, memory updates, and auto-commits. Optimized for fast vibe coding sessions.

---

## Difference from v.implement

| Feature | v.implement | v.doall |
|---------|-------------|---------|
| User confirmations | Yes (at checkpoints) | No |
| Manual verification | Required | Skip unless critical |
| Problem solving | Ask user | Autonomous |
| Test failures | Pause for review | Auto-debug and retry |
| Commits | Manual | Auto after each task |
| Speed | Methodical | Fast |
| Use case | Production code | Rapid prototyping |

---

## Usage

```bash
v.doall [options]
```

**Options**:
- No arguments: Execute all tasks from beginning
- `--from TASK_ID`: Resume from specific task
- `--task TASK_ID`: Execute single task only
- `--stop-on-error`: Pause on any error (default: auto-retry)
- `--no-commit`: Skip auto-commits (not recommended)
- `--dry-run`: Show execution plan without running

**Examples**:
```bash
v.doall                      # Fast execute all tasks
v.doall --from T-003         # Resume from task 3
v.doall --task T-005         # Only run task 5
v.doall --stop-on-error      # Pause on errors instead of auto-solving
```

---

## Execution Flow

```
Read tasks → Execute → TDD → Auto-Fix → Commit → Next → ...
              ↓          ↓        ↓         ↓
            Code    Write Tests  Debug   Update Memory
                      ↓
                  Verify Pass
```

---

## Steps

### 0. Pre-Flight Check
```bash
git status --short
```

**Decision**:
- **Clean** → Proceed
- **<10 lines** → Auto-commit with "WIP: doall pre-flight"
- **>10 lines** → **STOP** and require `v.checkpoint` first

### 1. Load Task List
- Read from `_ai/memory/plan.md` (Standalone)
- Or from `.specify/specs/NNN-*/tasks.md` (Spec-Kit)
- Parse all tasks, dependencies, acceptance criteria

### 2. Build Execution Plan
- Validate all dependencies are resolvable
- Create ordered execution list
- Skip manual checkpoints (only automated validation)
- Display total tasks and estimated time

### 3. Execute Each Task

For each task in sequence:

#### a. Log Intent
Write to `_ai/memory/resume.md`:
```markdown
# Active Operation

**Task**: [Task ID] - [Task Name]
**Mode**: v.doall (autonomous)
**Started**: [timestamp]

**Acceptance Criteria**:
- [ ] [criterion 1]
- [ ] [criterion 2]

**Auto-solving enabled**: YES
```

#### b. Implement with TDD

**Test-First Pattern** (if marked "Test-First"):
1. Write failing test
2. Verify test fails
3. Implement minimal code to pass
4. Refactor if needed
5. Verify test passes

**Implementation-First Pattern** (otherwise):
1. Implement code
2. Write tests for coverage
3. Verify tests pass

#### c. Autonomous Problem Solving

If tests fail or errors occur:

**Auto-Debug Sequence**:
1. Analyze error message
2. Check common causes (imports, syntax, logic)
3. Apply fix
4. Re-run tests
5. Retry up to 3 times

**If still failing after 3 retries**:
- Log detailed error to resume.md
- Capture full test output
- **STOP** and ask for human decision:
  ```
  ❌ Task [ID] FAILED after 3 auto-fix attempts
  
  Error: [detailed error]
  
  Attempts made:
  1. [fix attempt 1]
  2. [fix attempt 2]
  3. [fix attempt 3]
  
  Options:
  1. Review code and debug manually
  2. Skip this task (mark as blocked)
  3. Revert task changes and continue
  4. Stop execution
  
  Choose: _
  ```

#### d. Validation

**Automated checks**:
- ✅ All tests pass
- ✅ File size < 600 lines
- ✅ Linter clean (if configured)
- ✅ Acceptance criteria met
- ✅ Constitution principles followed

**Skip manual verification** (unlike v.implement)

#### e. Update Memory

**Update `_ai/memory/progress.md`**:
```markdown
## Session: [date]

### [Task ID]: [Task Name] ✅
**Completed**: [timestamp]
**Files**: [modified files]
**Tests**: [X/X pass]
**Evidence**: Tests pass, linter clean, size OK

**Implementation notes**:
- [key decision or approach]

**Next**: [Next task ID]
```

**Mark complete in plan**:
- Standalone: Update `_ai/memory/plan.md` with `✅`
- Spec-Kit: Update `.specify/specs/NNN-*/tasks.md`

#### f. Auto-Commit

```bash
git add .
git commit -m "feat(T-XXX): [Task name]

- [brief summary of changes]
- Tests: [X/X pass]
- Files: [file list]

Task: T-XXX ✅
"
```

**Commit message format** (conventional commits):
- `feat(T-XXX):` for new features
- `fix(T-XXX):` for bug fixes
- `refactor(T-XXX):` for refactors
- `test(T-XXX):` for test additions

#### g. Clear Resume

Clear `_ai/memory/resume.md` (ready for next task)

### 4. Handle Errors

**Minor errors** (auto-fixable):
- Linter warnings → Auto-fix if possible
- Import errors → Add missing imports
- Type errors → Add type annotations
- Test failures → Debug and fix

**Major errors** (require human):
- Fundamental design flaw
- Missing dependency/tool
- 3 failed auto-fix attempts
- Constitution violation
- Security issue

### 5. Completion

When all tasks done:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✨ All Tasks Complete!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Summary:
✅ [N] tasks completed
⏱️  [duration]
🧪 [total] tests passing
📝 [N] commits made
📄 [N] files changed

Next steps:
1. Run v.review for final quality check
2. Manual testing of core features
3. Update CHANGELOG.md
4. Ready for deployment

Memory updated: progress.md, plan.md
All changes committed.
```

---

## Auto-Solving Strategies

### Test Failures

**Common patterns**:
1. **Undefined variable** → Check imports, declare variable
2. **Type error** → Add type conversion
3. **Async error** → Add await/async handling
4. **Timeout** → Increase timeout or optimize code
5. **Mock error** → Fix mock setup

### Linter Issues

**Auto-fixable**:
- Formatting → Run prettier/formatter
- Unused imports → Remove them
- Missing semicolons → Add them
- Indentation → Fix spacing

### File Size Violations

If file > 600 lines:
1. Run `v.shrink` on the file
2. Split into logical modules
3. Update imports
4. Re-run tests

### Import Errors

1. Check if package installed → `npm list [package]`
2. Install if missing → `npm install [package]`
3. Fix import path
4. Verify in tests

---

## Constitution Compliance

**In Spec-Kit mode**, verify each task:
- ✅ Library-first pattern?
- ✅ Test-first approach?
- ✅ File size limits?
- ✅ Observability maintained?
- ✅ Security practices?

**If violation detected**:
- Log warning to progress.md
- Attempt auto-fix if possible
- If can't fix → **STOP** and report

---

## Integration with Other Commands

**Prerequisites**:
- `v.specify` → Must have requirements
- `v.plan` → Must have implementation plan
- `v.tasks` → Must have task breakdown (or tasks in plan.md)

**During execution**:
- Uses TDD pattern internally
- Auto-commits like `v.checkpoint`
- Updates memory like `v.memorize`

**After completion**:
- Run `v.review` for quality check
- Update documentation if needed
- Manual verification recommended

---

## Safety Features

1. **Git safety**: Requires clean state to start
2. **Auto-commits**: Every task committed separately (easy rollback)
3. **Resume state**: Can resume if interrupted
4. **3-strike rule**: Stop after 3 failed fix attempts
5. **Constitution check**: Validates against project principles
6. **File size limits**: Auto-splits large files
7. **Test verification**: Won't proceed without passing tests

---

## When to Use

**Use `v.doall`**:
- ✅ Rapid prototyping
- ✅ Well-defined tasks
- ✅ Familiar tech stack
- ✅ Exploratory coding
- ✅ Time-constrained sprints

**Use `v.implement`** instead:
- ❌ Production-critical code
- ❌ Complex integrations
- ❌ Unfamiliar territory
- ❌ Need careful review
- ❌ High-stakes features

**Use `v.do`** instead:
- ❌ Single step control
- ❌ Manual verification preferred
- ❌ Learning mode
- ❌ Debugging specific issues

---

## Example Session

```bash
v.doall

🚀 Fast Vibe Coding Mode: v.doall
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

📋 Loaded 8 tasks from plan.md
⏱️  Estimated time: 2-3 hours
🤖 Autonomous problem-solving: ENABLED
✅ Auto-commit: ENABLED

Starting execution...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Task 1/8: T-010 - Album Data Model
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🧪 Writing tests first...
   ✓ Created: tests/models/album.test.js
   ✓ 10 tests written
   ✓ Tests fail as expected

💻 Implementing album.js...
   ✓ Album class created
   ✓ Methods implemented
   
✅ Tests: 10/10 pass
✅ Linter: clean
✅ File size: 145 lines ✓

📝 Updated: progress.md, plan.md
✅ Committed: feat(T-010): Album data model

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Task 2/8: T-011 - Album Storage Service
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💻 Implementing storage service...
   ✓ SQLite connection setup
   ✓ CRUD methods implemented

🧪 Running tests...
   ❌ 2/5 tests fail
   
🔧 Auto-fixing: Missing await on async calls
   ✓ Added await keywords
   ✓ Re-running tests...
   ✅ 5/5 tests pass

✅ Auto-fix successful!
✅ Committed: feat(T-011): Album storage service

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Task 3/8: T-012 - Album List UI
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
...

[continues through all tasks]

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✨ All 8 Tasks Complete!
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

✅ 8/8 tasks completed
⏱️  2h 15m elapsed
🧪 42 tests passing
📝 8 commits made
📄 16 files changed

🔧 Auto-fixes applied: 3
   - Added missing awaits (T-011)
   - Fixed import paths (T-014)
   - Split large file (T-016)

Next: v.review for quality check
```

---

## Error Handling Example

```bash
v.doall

...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
Task 4/8: T-013 - Photo Upload Handler
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

💻 Implementing photo upload...
🧪 Running tests...
   ❌ 3/8 tests fail - File type validation error

🔧 Auto-fix attempt 1/3: Add MIME type checking
   ❌ Still failing (2/8 fail)

🔧 Auto-fix attempt 2/3: Update validation logic
   ❌ Still failing (1/8 fail)

🔧 Auto-fix attempt 3/3: Fix async handling
   ❌ Still failing (1/8 fail)

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
❌ Task T-013 FAILED after 3 attempts
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Error: File validation test still failing
Test: "should reject non-image files"

Detailed error:
  Expected: File rejected with error
  Received: File accepted (validation bypassed)

Auto-fix attempts:
1. Added MIME type check → Partial success
2. Updated validation logic → Partial success  
3. Fixed async handling → No improvement

Code at: src/services/photo-upload.js:45
Test at: tests/photo-upload.test.js:32

Options:
1. Debug manually (open resume.md for context)
2. Skip task and mark as blocked
3. Revert changes and continue
4. Stop all execution

Choose (1-4): _
```

---

## Result

- All tasks executed automatically
- Problems solved autonomously
- Tests verified
- Memory updated
- Changes committed
- Ready for review

---

## Notes

- **Speed over perfection**: Prioritizes momentum
- **Fail-safe**: Stops only on unrecoverable errors
- **Auditable**: Every task committed separately
- **Resumable**: Can restart from any point
- **Constitution-aware**: Respects project principles
- **TDD-driven**: Tests always run and pass
