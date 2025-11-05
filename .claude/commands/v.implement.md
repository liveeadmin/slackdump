# v.implement — Execute Implementation Plan

**Purpose**: Automatically execute all tasks from task list in correct order, respecting dependencies, parallelization, and checkpoints.

---

## Usage

```bash
v.implement [options]
```

**Options**:
- No arguments: Execute all tasks from beginning
- `--from TASK_ID`: Resume from specific task
- `--task TASK_ID`: Execute single task only
- `--dry-run`: Show what would be executed without doing it
- `--parallel`: Allow parallel task execution (default: sequential)

**Examples**:
```bash
v.implement                    # Execute all tasks
v.implement --from Task-2.1    # Resume from Task 2.1
v.implement --task Task-1.2    # Execute only Task 1.2
v.implement --dry-run          # Preview execution plan
```

---

## Behavior

This command is the automated execution engine for the entire workflow:

1. Reads task list (tasks.md or plan.md)
2. Validates prerequisites (spec, plan, constitution if required)
3. Executes tasks in dependency order
4. Runs checkpoints for validation
5. Updates progress automatically
6. Handles errors and rollback

---

## Execution Flow

```
Read tasks.md → Validate → Execute → Checkpoint → Next Task → ...
     ↓                        ↓           ↓
   Parse               Test-First    Validate      
   Dependencies        Pattern       Acceptance
```

### Detailed Steps

1. **Load Task List**
   - Read from `.specify/specs/NNN-*/tasks.md` (Spec-Kit)
   - Or read from `_ai/memory/plan.md` (Standalone)
   - Parse tasks, dependencies, parallel markers

2. **Validate Prerequisites**
   - Verify spec exists
   - Verify plan exists
   - Check constitution (if Spec-Kit mode)
   - Confirm all dependencies resolvable

3. **Build Execution Graph**
   - Create dependency graph
   - Identify parallel execution groups
   - Find checkpoint boundaries
   - Determine execution order

4. **Execute Tasks Sequentially**
   
   For each task:
   
   a. **Pre-execution**
      - Log to `_ai/memory/resume.md`
      - Display task details
      - Show acceptance criteria
   
   b. **Test-First Check**
      - If task marked "Test-First"
      - Write tests first
      - Verify tests fail
      - Get user approval
   
   c. **Implementation**
      - Read task steps
      - Implement code changes
      - Follow file size limits
      - Respect constitution principles
   
   d. **Validation**
      - Run tests (if exist)
      - Run linter (if configured)
      - Check file sizes
      - Verify acceptance criteria
   
   e. **Post-execution**
      - Update progress.md
      - Mark task complete
      - Clear resume.md
      - Commit if requested

5. **Handle Checkpoints**
   - Run all validation steps
   - Manual verification prompts
   - Integration tests
   - Acceptance criteria check
   - Pause for user confirmation

6. **Parallel Execution** (if `--parallel` flag)
   - Identify tasks marked `[P]`
   - Execute in parallel threads
   - Wait for all to complete
   - Aggregate results

7. **Error Handling**
   - Log error to resume.md
   - Offer rollback option
   - Suggest fix or skip
   - Resume from last good state

---

## Task Execution Detail

### For Each Task:

```markdown
Task 1.2: Album Storage Service
Files: src/services/album-storage.js, tests/album-storage.test.js
Type: Test-First
```

**Execution**:

1. **Display**:
   ```
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   📋 Task 1.2: Album Storage Service
   📁 Files: album-storage.js, album-storage.test.js
   ✓ Dependencies met: Task 1.1 complete
   ━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
   ```

2. **Test-First**:
   ```
   🧪 Writing tests first (Test-First pattern)...
   
   Created: tests/services/album-storage.test.js
   
   describe('AlbumStorage', () => {
     test('saves album to database', async () => {
       // Test code
     });
   });
   
   Running tests... ❌ FAIL (expected)
   
   ✓ Tests fail as expected
   Press Enter to continue with implementation...
   ```

3. **Implementation**:
   ```
   💻 Implementing album-storage.js...
   
   - Created SQLite connection
   - Implemented save() method
   - Implemented load() method
   - Added error handling
   
   ✓ Implementation complete
   ```

4. **Validation**:
   ```
   ✅ Validating...
   
   - Running tests... ✓ 5/5 pass
   - Checking file size... ✓ 145 lines (< 600)
   - Running linter... ✓ No issues
   - Checking acceptance... ✓ All criteria met
   
   ✓ Task 1.2 complete!
   ```

5. **Update Progress**:
   ```
   Updated: _ai/memory/progress.md
   Updated: .specify/specs/001-*/tasks.md
   
   ✅ Task 1.2: Album Storage Service — Complete
      Evidence: Tests pass (5/5), file size OK, linter clean
   ```

---

## Checkpoint Execution

When reaching a checkpoint:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚦 Checkpoint 1: US-001 Validation
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Automated Checks:
✓ All US-001 tasks complete
✓ All tests passing (15/15)
✓ No file size violations
✓ Linter clean

Manual Verification Required:
□ Create album via UI
□ Verify album persists after refresh
□ Test drag-and-drop reordering

Please perform manual verification and confirm:
> All checks pass? (y/n): _
```

---

## Integration with Constitution

If constitution exists, validate each task:

- ✅ Follows library-first pattern?
- ✅ Tests written first?
- ✅ File size under limit?
- ✅ Observability maintained?
- ✅ Security practices followed?

Stop and warn if violations detected.

---

## Progress Tracking

### In progress.md:
```markdown
## Current Session: 2025-11-04

### Implementing: Photo Organizer (US-001)

✅ Task 1.1: Album Data Model — Complete (tests: 10/10, size: 148 lines)
✅ Task 1.2: Album Storage — Complete (tests: 5/5, size: 145 lines)
🔄 Task 1.3: Album List UI — In Progress
   Step 2/4: Implementing CSS for tile layout
   
Next: Complete Task 1.3, then Checkpoint 1
```

### In resume.md:
```markdown
# Active Operation

**Task**: 1.3 Album List UI
**File**: src/components/album-list.js
**Step**: 2/4 - Implementing CSS layout
**Intent**: Create responsive tile grid for albums

**Micro-steps**:
1. ✅ Created HTML structure
2. 🔄 Implementing CSS grid layout
3. ⏳ Add JavaScript rendering
4. ⏳ Write UI tests

**Acceptance**:
- Albums display in grid
- Responsive layout
- Click handler works
- UI tests pass
```

---

## Error Recovery

If task fails:

```
❌ Task 1.2 FAILED

Error: Tests not passing (3/5 fail)

Options:
1. View failing tests
2. Debug interactively
3. Rollback changes
4. Skip this task (not recommended)
5. Pause and resume later

Choose option (1-5): _
```

---

## Parallel Execution Example

```bash
v.implement --parallel

# Execution plan:
# Sequential: Task 1.1
# Parallel:   Task 1.2 ⟷ Task 1.3
# Sequential: Checkpoint 1
# Sequential: Task 2.1
# ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔀 Parallel Execution: Tasks 1.2 & 1.3
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Thread 1: Task 1.2 Album Storage...
Thread 2: Task 1.3 Album List UI...

⏳ Waiting for both tasks to complete...

✓ Task 1.2 complete (2m 34s)
✓ Task 1.3 complete (3m 12s)

All parallel tasks complete! Proceeding...
```

---

## Integration with Other Commands

**Prerequisites**:
- `v.specify` — Must have specification
- `v.plan` — Must have implementation plan
- `v.tasks` — Must have task breakdown

**During execution**:
- Uses `v.do` internally for each task
- Calls `v.checkpoint` at checkpoints
- Updates with `v.memorize` pattern

**After completion**:
- Run `v.review` for final validation
- Run `v.checkpoint` for clean commit
- Document in CHANGELOG

---

## Dry Run Example

```bash
v.implement --dry-run

Execution Plan:
==============

Phase 1: Foundation
  ✓ Task 1.1: Album Data Model (0 dependencies)
    Files: album.js, album.test.js
    Estimated: 30 minutes
  
  ✓ Task 1.2: Album Storage [P] (depends: 1.1)
    Files: album-storage.js, album-storage.test.js
    Estimated: 45 minutes
  
  ✓ Task 1.3: Album List UI [P] (depends: 1.1)
    Files: album-list.js, album-list.css
    Estimated: 1 hour
  
  ⚡ Tasks 1.2 & 1.3 can run in parallel
  
  ⏸ Checkpoint 1: US-001 Validation
    Manual verification required
  
Phase 2: Enhancements
  ✓ Task 2.1: Drag-Drop Service (depends: Checkpoint 1)
    ...

Total tasks: 8
Estimated time: 6 hours (4 hours with parallel)
Checkpoints: 2

Ready to execute? (y/n): _
```

---

## Result

- All tasks executed in order
- Tests passing
- Acceptance criteria met
- Progress documented
- Code committed
- Feature complete

## Notes

- `v.implement` is the "autopilot" mode
- Use `v.next` + `v.do` for manual control
- Parallel execution requires `--parallel` flag
- Checkpoints always pause for verification
- Constitution violations stop execution
- Can resume from any task if interrupted
