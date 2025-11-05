# v.next — What to Do Next

**Purpose**: Read plan and progress, then output the next executable step and its acceptance criteria.

---

## Mode Detection & Path Resolution

### Spec-Kit Mode (if `.specify/` exists)
1. Detect active feature from git branch name (e.g., `003-user-auth`)
2. Read `.specify/specs/003-user-auth/tasks.md` for task list
3. Read `.specify/specs/003-user-auth/plan.md` for context
4. Session progress still in `_ai/memory/progress.md` (optional)

### Standalone Mode
1. Read `_ai/memory/progress.md` for current state
2. Read `_ai/memory/plan.md` for phases/tasks/steps

---

## Steps
1. **Detect Mode**: Check if `.specify/` directory exists
2. **Load Context**: 
   - Spec-Kit: Read from `.specify/specs/NNN-feature/`
   - Standalone: Read from `_ai/memory/`
3. **Determine Next Action**:
   - Current phase
   - First unchecked task in order (or the one marked in-progress)
   - The next step (S-xxxx or task number) to execute
4. **Output**: One actionable step with acceptance checks and affected files
5. **Handle Blockers**: If blocked, surface blocker and suggest micro-unblock task

---

## Result
- A single actionable step with acceptance criteria and file pointers
- Works seamlessly in both Spec-Kit and Standalone modes
