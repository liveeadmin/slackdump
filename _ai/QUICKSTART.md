# QUICKSTART — Vibe-Coding Framework
**Version**: 1.0.0 | **Last Updated**: 2025-11-04

**Owner**: VibeLogic.app — Confidential / Proprietary

## 1) New Project
```text
v.createprd     # write _ai/memory/prd.md (goals, MVP, constraints)
v.initproject   # generate plan.md, progress.md, resume.md, techenv.md, changelog, quickstart
v.next          # get the next executable step (S-xxxx + acceptance)
v.do            # implement the step; resume.md/progress.md kept in sync
```

## 2) Daily Loop
```text
v.next  → v.do  → (repeat)
# When pausing or stabilizing:
v.checkpoint    # runs v.shrink + v.testsync + build, then v.memorize and commit
```

## 🧠 Legacy Migration

If you are migrating an existing project:

```bash
# 1. Copy your old memory or documentation files
cp old_project/docs/* _ai/memory/legacy/

# 2. Import them into the new system
v.initmemory

# 3. Summarize and commit the merged state
v.memorize
v.checkpoint

# 4. (Optional) Regenerate docs if structure changed
v.initproject
```

Then continue normally:
```bash
v.next → v.do → v.checkpoint
```

## 4) Handy Tools
```text
v.shrink     # enforce 600-line cap and split plans
v.testsync   # fix test signatures to compile; list behavioral failures
v.memorize   # sync progress + changelog; rotate archives
v.whatif     # evaluate ideas; add to plan if accepted
v.review     # lint/format/security/readability pass
v.resume     # continue last active operation
v.archive    # move bulky history to archive/ and keep pointers
v.syncdocs   # update README/QUICKSTART snippets from current rules
```

## 5) Quality Bar
- One active step at a time; follow plan order.
- 600-line hard cap per file (target 200–400).
- Tests and lint pass before marking “done.”
- No secrets in repo; safe logging.

© VibeLogic.app — Confidential / Proprietary — Do Not Redistribute
