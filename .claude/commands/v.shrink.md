# v.shrink — File Size Optimization

**Purpose**: Detect and refactor oversized source files using the 600-line hard cap policy.

---

## Steps
1. Run repo size check (per-extension thresholds).
2. For each violation, create a split plan and extract responsibilities into new files (same package).
3. Re-run build/tests; iterate until all files < 600 lines.
4. Document summary in _ai/memory/progress.md; link details in _ai/memory/archive/.
5. Prefer destination dir memory/refactoring/<ext>/ for temporary staging if moving files during refactor.

---

## Result
- No file exceeds 600 lines.
- Refactoring plans archived; progress updated.
