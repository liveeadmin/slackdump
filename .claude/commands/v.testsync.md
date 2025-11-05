# v.testsync — Test Compliance Sync

**Purpose**: Make test files compile with current APIs; separate behavioral failures for later debugging.

---

## Steps
1. Find and compile tests; capture errors.
2. Apply mechanical fixes: renamed types, added/removed params, moved methods/imports.
3. Recompile; repeat until tests compile.
4. Run tests; list failing cases as behavioral (no auto-fix).
5. Commit test signature updates separately if substantial.

---

## Result
- Tests compile.
- Behavioral failures enumerated for follow-up.
