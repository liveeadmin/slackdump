# v.resume — Reload Last Active Operation

**Purpose**: Open the single entry in _ai/memory/resume.md and continue exactly where it left off.

---

## Steps
1. Read _ai/memory/resume.md: intent, scope, micro-steps.
2. Validate files still match scope; rebuild if needed.
3. Execute micro-steps or refine them if stale.
4. On completion, mark DONE with commit hash and clear the active slot.

---

## Result
- Work resumes with zero thrash.
- Resume log closed or updated.
