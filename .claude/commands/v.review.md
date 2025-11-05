# v.review — Code Quality & Security Pass

**Purpose**: Lightweight pre-commit quality sweep rooted in clarity, safety, and minimalism.

---

## Steps
1. Lint/format; remove dead code and unused deps.
2. Security check: no secrets; validate inputs; safe logging; permissions least-privilege.
3. Readability check: short functions; self-explanatory names; no cleverness.
4. Docs: public APIs have brief comments; update README/QUICKSTART only if user-facing changed.
5. Rerun unit tests; ensure fast, hermetic defaults.

---

## Result
- Code clearer and safer.
- No secrets or noisy logs.
- Tests still green.
