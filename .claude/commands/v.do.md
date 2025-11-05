# v.do — Execute Current Step

**Version**: 2.0.0 | **Last Updated**: 2025-11-04

**Purpose**: Start or continue the next step from plan.md. Keep scope tight and log to resume.md. Works in both Standalone and Spec-Kit modes.

---

## Mode Detection

### Spec-Kit Mode (if `.specify/` exists)
- Read tasks from `.specify/specs/NNN-feature/tasks.md`
- Read plan from `.specify/specs/NNN-feature/plan.md`
- Follow constitution `.specify/memory/constitution.md` principles
- Use `_ai/memory/resume.md` for session state

### Standalone Mode
- Read from `_ai/memory/plan.md`
- Use `_ai/memory/progress.md` and `_ai/memory/resume.md`

---

## Steps

### 0. Pre-Flight Git Safety Check (MANDATORY)
```bash
git status --short
```

**Decision logic**:
- **Clean working directory** → Proceed to step 1
- **≤10 lines changed** → Proceed with caution, mention in output
- **>10 lines uncommitted** → **STOP** and suggest:
  ```
  ⚠️  Uncommitted changes detected (>10 lines). 
  Recommend running v.checkpoint first to avoid losing work.
  
  Options:
  1. Run v.checkpoint to commit current work
  2. Review changes: git diff --stat
  3. Override and proceed (not recommended)
  ```
- **Untracked files** → List them and ask if they should be committed

### 1. Detect Mode
Check for `.specify/` directory to determine operating mode

### 2. Identify Next Step
Via `v.next` (adapts to mode); display acceptance criteria

### 3. Update Resume
Write to `_ai/memory/resume.md` with intent, scope, and 1–3 micro-steps

### 4. Check Principles
In Spec-Kit mode, verify alignment with constitution.md

### 5. Implement
Make changes; keep commits small and scoped

### 6. Verify
Run fast tests/lint; verify acceptance

### 7. On Success
- Tick step/task in plan.md or tasks.md (based on mode)
- Update progress.md (state/next/decision if any)
- Clear resume.md
- **Checkpoint Reminder**:
  - After 3 steps: "💡 Consider v.checkpoint"
  - After task completion: "✅ Task done! Run v.checkpoint to commit"
  - After large refactor: "⚠️  Recommend v.checkpoint now"

### 8. On Interruption
Leave resume.md populated (single open entry)

---

## Result
- Step completed or clearly parked
- Progress and plan updated in correct locations
- Resume state consistent
- Constitution respected (in Spec-Kit mode)
