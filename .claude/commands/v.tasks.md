# v.tasks — Generate Task Breakdown

**Purpose**: Break implementation plan into specific, actionable, ordered tasks with dependencies and acceptance criteria.

---

## Usage

```bash
v.tasks
```

No arguments needed — reads from existing plan.

---

## Behavior by Mode

### Spec-Kit Mode (if `.specify/specs/NNN-*/plan.md` exists)
Creates `.specify/specs/NNN-feature/tasks.md` with:
- Tasks organized by user story
- Dependency markers
- Parallel execution flags `[P]`
- File path specifications
- Test-first task ordering
- Checkpoint validation tasks

### Standalone Mode
Enhances `_ai/memory/plan.md` with:
- Detailed step breakdown
- Execution order
- Acceptance criteria per step
- File references

---

## Task Breakdown Structure

### Full Task List (Spec-Kit Mode)

```markdown
# Task Breakdown: [Feature Name]

## User Story: US-001 - [Story Title]

### Task 1.1: [Task Name]
**Files**: src/models/album.js, tests/album.test.js
**Dependencies**: None
**Type**: Test-First

**Steps**:
1. Write test for Album model creation
2. Implement Album class with properties
3. Verify tests pass

**Acceptance**:
- [ ] Tests written and fail initially
- [ ] Album model implemented
- [ ] All tests pass
- [ ] File size < 200 lines

### Task 1.2: [Task Name] [P]
**Files**: src/services/album-service.js
**Dependencies**: Task 1.1
**Type**: Implementation
**Parallel**: Can run in parallel with Task 1.3

**Steps**:
...

### Task 1.3: [Task Name] [P]
**Files**: src/ui/album-list.js
**Dependencies**: Task 1.1
**Type**: UI Component
**Parallel**: Can run in parallel with Task 1.2

**Steps**:
...

### Checkpoint 1: US-001 Validation
**Type**: Validation
**Dependencies**: All US-001 tasks

**Validation Steps**:
- [ ] All US-001 acceptance criteria met
- [ ] Integration tests pass
- [ ] Manual verification complete
- [ ] Ready for next user story

---

## User Story: US-002 - [Next Story]
...
```

### Lightweight Task List (Standalone Mode)

```markdown
## PH-1: Core Features

### T-010: Create Album Model
**Step S-010-A**: Write Album test suite
**Step S-010-B**: Implement Album class
**Step S-010-C**: Verify tests pass
**Files**: src/models/album.js, tests/album.test.js
**Acceptance**: Tests pass, model complete

### T-011: Build Album Service
**Depends on**: T-010
**Step S-011-A**: Define service interface
...
```

---

## Steps

1. **Read Implementation Plan**
   - Load plan.md from appropriate location
   - Extract phases and components
   - Identify user stories (if Spec-Kit mode)
   - Note tech stack and architecture

2. **Map Requirements to Tasks**
   - Each user story → set of tasks
   - Each technical requirement → supporting tasks
   - Group related tasks together

3. **Order by Dependencies**
   - Models before services
   - Services before UI
   - Core before features
   - Setup before implementation

4. **Apply Test-First Pattern**
   - Test task precedes implementation task
   - Both tasks reference same files
   - Acceptance includes "tests pass"

5. **Identify Parallelization**
   - Mark tasks that can run concurrently with `[P]`
   - Independent UI components
   - Separate services
   - Different user stories

6. **Add Checkpoints**
   - After each user story
   - Before major integrations
   - At phase boundaries
   - Validation steps included

7. **Specify Files**
   - Exact file paths for each task
   - Test files paired with implementation
   - Keep files under 600 lines

8. **Write Acceptance Criteria**
   - Specific, testable conditions
   - Evidence of completion
   - Quality gates (tests, lint, size)

9. **Save Task List**
   - Write to appropriate location
   - Format for consumption by `v.next` and `v.implement`

---

## Task Organization Patterns

### By User Story (Spec-Kit)
```
US-001: Create Albums
├─ Task 1.1: Album Model (test-first)
├─ Task 1.2: Album Service [P]
├─ Task 1.3: Album UI [P]
└─ Checkpoint 1: US-001 Validation

US-002: Organize Albums
├─ Task 2.1: Drag-Drop Service
├─ Task 2.2: Reorder Logic
└─ Checkpoint 2: US-002 Validation
```

### By Phase (Standalone)
```
PH-0: Setup
├─ T-001: Project scaffold
├─ T-002: Database setup
└─ T-003: Build config

PH-1: Core Models
├─ T-010: Album model
├─ T-011: Photo model
└─ T-012: Tag model
```

---

## Dependency Management

### Types of Dependencies

**Sequential** (must wait):
```
Task 1.1: Album Model
  ↓ (depends on)
Task 1.2: Album Service
  ↓ (depends on)
Task 1.3: Album UI
```

**Parallel** (can run together):
```
Task 1.2: Album Service [P] ⟷ Task 1.3: Album UI [P]
(both depend on Task 1.1, but independent of each other)
```

**Checkpoint** (all must complete):
```
Task 2.1 ─┐
Task 2.2 ─┼─→ Checkpoint: Phase 2 Complete
Task 2.3 ─┘
```

---

## Integration with v.implement

The task list format is designed for automated execution:

```bash
v.implement

# Reads tasks.md (or plan.md)
# Executes in order:
# 1. Task 1.1 (sequential)
# 2. Tasks 1.2 & 1.3 (parallel)
# 3. Checkpoint 1 (validation)
# 4. Task 2.1 (sequential)
# ...
```

---

## Example Output

```markdown
# Task Breakdown: Photo Organizer

## User Story: US-001 - Create Photo Albums

### Task 1.1: Album Data Model
**Files**: 
- src/models/album.js
- tests/models/album.test.js

**Dependencies**: None
**Type**: Test-First

**Steps**:
1. Create test file with Album creation tests
2. Define Album class with id, name, date, photos[]
3. Implement methods: addPhoto(), removePhoto(), getPhotoCount()
4. Run tests and verify all pass

**Acceptance**:
- [ ] Tests written and initially fail
- [ ] Album class implemented with all methods
- [ ] All 10+ tests pass
- [ ] File size < 150 lines

### Task 1.2: Album Storage Service [P]
**Files**:
- src/services/album-storage.js
- tests/services/album-storage.test.js

**Dependencies**: Task 1.1
**Type**: Test-First
**Parallel**: Can run with Task 1.3

**Steps**:
1. Write tests for save/load/delete operations
2. Implement SQLite storage using sql.js
3. Add error handling and validation
4. Verify tests pass

**Acceptance**:
- [ ] SQLite integration working
- [ ] CRUD operations tested
- [ ] Error cases handled
- [ ] Tests pass

### Task 1.3: Album List UI [P]
**Files**:
- src/components/album-list.js
- src/components/album-list.css
- tests/components/album-list.test.js

**Dependencies**: Task 1.1
**Type**: UI Component
**Parallel**: Can run with Task 1.2

**Steps**:
1. Create HTML structure for album grid
2. Implement CSS for tile layout
3. Add JavaScript for rendering albums
4. Write UI tests for rendering

**Acceptance**:
- [ ] Albums display in grid
- [ ] Responsive layout works
- [ ] Clicking album opens it
- [ ] UI tests pass

### Checkpoint 1: US-001 Validation
**Type**: Validation
**Dependencies**: Tasks 1.1, 1.2, 1.3

**Validation Steps**:
1. Manual test: Create album via UI
2. Verify album persists after refresh
3. Confirm acceptance criteria from spec.md
4. Run all tests in US-001

**Acceptance**:
- [ ] All US-001 criteria met
- [ ] Integration works end-to-end
- [ ] No console errors
- [ ] Ready for US-002
```

---

## Integration with Other Commands

**After task generation**:
- Run `v.implement` for automated execution
- Or use `v.next` → `v.do` for manual control
- Use `v.checkpoint` at each checkpoint

**Before task generation**:
- Must have `v.specify` (requirements)
- Must have `v.plan` (tech stack)
- Optional: `v.constitution` (for patterns)

---

## Result

- Detailed task breakdown
- Clear execution order
- Dependency relationships
- Parallelization opportunities
- Acceptance criteria per task
- Ready for implementation

## Notes

- Tasks are **actionable** (can be executed)
- Tasks are **testable** (clear acceptance)
- Tasks are **ordered** (dependencies respected)
- Tasks are **sized** (hours, not days)
- Test-first pattern enforced where applicable
