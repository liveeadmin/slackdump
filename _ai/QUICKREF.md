# Quick Reference — Dual-Mode Commands
**Version**: 1.1.0 | **Last Updated**: 2025-11-04


## Installation (Optional)

To use Spec-Kit CLI for bootstrapping new projects:

```bash
# Persistent installation (recommended)
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git

# One-time usage
uvx --from git+https://github.com/github/spec-kit.git specify init <PROJECT_NAME>
```

**Note**: The Specify CLI is optional. Our `v.speckit` command provides similar functionality for existing projects.

## Mode Detection

Check current mode:
```bash
v.speckit status
```

## Command Quick Reference

### Initialization

| Scenario | Command | Result |
|----------|---------|--------|
| **Existing project** | `v.onboard [path]` | Analyzes & generates all memory files |
| Existing + deep analysis | `v.onboard --deep` | Full analysis + architecture docs |
| Existing + quick scan | `v.onboard --quick` | Basic structure only |
| New lightweight project | `v.specify` → `v.plan` → `v.tasks` | Standalone mode with `_ai/memory/` |
| New structured project | `v.speckit init` → `/speckit.constitution` | Spec-Kit mode with `.specify/` |
| Upgrade standalone → Spec-Kit | `v.speckit init` | Migrates files to `.specify/` |
| Legacy memory import | `v.initmemory` | Imports from `_ai/memory/legacy/` |

### Workflow Commands

| Task | v.* Command | /speckit.* Command | Notes |
|------|-------------|-------------------|-------|
| **Create constitution** | `v.constitution` | `/speckit.constitution` | Both work in either mode |
| **Define requirements** | `v.specify` | `/speckit.specify` | Focus on WHAT/WHY |
| **Clarify ambiguities** | Ad-hoc in v.specify | `/speckit.clarify` | Spec-Kit has structured process |
| **Create implementation plan** | `v.plan` | `/speckit.plan` | Specify tech stack (HOW) |
| **Generate task list** | `v.tasks` | `/speckit.tasks` | Detailed breakdown with dependencies |
| **Execute all tasks** | `v.implement` | `/speckit.implement` | Full automation |
| **Execute next task** | `v.next` → `v.do` | `v.next` → `v.do` | Manual control |
| **Checkpoint/commit** | `v.checkpoint` | `v.checkpoint` | Same command, adapts to mode |

### Memory & Documentation

| Task | Standalone | Spec-Kit | Notes |
|------|-----------|----------|-------|
| **Track progress** | `_ai/memory/progress.md` | Optional journal | Spec-Kit focuses on task completion |
| **Resume work** | `_ai/memory/resume.md` | Same | Both use resume.md |
| **Archive old work** | `v.memorize` | Feature-based | Different strategies |
| **Principles/rules** | `CLAUDE.md` | `.specify/memory/constitution.md` | Can coexist |

### Quality & Validation

| Task | Command | Mode Support |
|------|---------|--------------|
| **Clarify requirements** | `v.clarify` | Both (adapts) |
| **Validate consistency** | `v.analyze` | Both (adapts) |
| **Generate checklists** | `v.checklist [type]` | Both (adapts) |
| **Code review** | `v.review` | Both (adapts) |
| **Check file sizes** | `v.shrink` | Both |
| **Sync tests with code** | `v.testsync` | Both |
| **Update docs** | `v.syncdocs` | Standalone |

### Bridge & Sync

| Task | Command |
|------|---------|
| Check mode | `v.speckit status` |
| Convert to Spec-Kit | `v.speckit init` |
| Generate constitution | `v.speckit constitution` |
| Sync between modes | `v.speckit sync` |

## File Locations

### Standalone Mode (`_ai/memory/`)

| File | Purpose |
|------|---------|
| `prd.md` | Requirements (lightweight) |
| `plan.md` | Phases → Tasks → Steps |
| `progress.md` | Session journal (past tense) |
| `resume.md` | Active operation (present tense) |
| `techenv.md` | Toolchain info |
| `archive/T-NNN.md` | Task archives |

### Spec-Kit Mode (`.specify/`)

| File | Purpose |
|------|---------|
| `memory/constitution.md` | Project principles |
| `specs/NNN-name/spec.md` | Feature requirements |
| `specs/NNN-name/plan.md` | Implementation plan |
| `specs/NNN-name/tasks.md` | Task breakdown |
| `specs/NNN-name/research.md` | Tech research |
| `scripts/` | Automation helpers |

## Common Workflows

### Workflow 1: Quick Solo Project (Modern)

```bash
# 1. Define what to build
v.specify "Build a CLI tool that converts CSV to JSON"

# 2. Clarify ambiguities (optional)
v.clarify

# 3. Create implementation plan
v.plan "Use Python with minimal dependencies, output to stdout"

# 4. Generate tasks
v.tasks

# 5. Validate consistency (optional)
v.analyze

# 6. Execute automatically
v.implement

# Or execute manually:
# v.next → v.do → v.next → v.do ...

# 7. Checkpoint when stable
v.checkpoint
```

### Workflow 1b: Quick Solo Project (Legacy)

```bash
# 1. Define what to build
v.createprd "Build a CLI tool that..."  # (aliased to v.specify)

# 2. Initialize project structure
v.initproject

# 3. Execute tasks
v.next      # See next task
v.do        # Execute it

# 4. Checkpoint when stable
v.checkpoint
```

### Workflow 2: Team Feature (Using v.* commands, with QA)

```bash
# 1. Set up principles (once per project)
v.constitution Create principles for code quality, testing, security

# 2. Define feature
v.specify Build shopping cart with real-time inventory

# 3. Validate spec quality
v.checklist spec
# (review and fix issues)

# 4. Clarify ambiguities
v.clarify

# 5. Plan implementation
v.plan Use React + TypeScript, Node.js backend, PostgreSQL

# 6. Generate tasks
v.tasks

# 7. Validate consistency
v.analyze
# (fix any issues identified)

# 8. Implement
v.implement
```

### Workflow 2b: Team Feature (Using /speckit.* commands)

```bash
# 1. Set up principles
/speckit.constitution Create principles for...

# 2. Define feature
/speckit.specify Build shopping cart with...

# 3. Clarify requirements (Spec-Kit structured clarification)
/speckit.clarify

# 4. Plan implementation
/speckit.plan Use React + Node.js...

# 5. Generate tasks
/speckit.tasks

# 6. Implement
/speckit.implement
```

### Workflow 3: Upgrade Existing Project

```bash
# 1. Check current state
v.speckit status
# Output: "Mode: Standalone"

# 2. Migrate to Spec-Kit
v.speckit init
# Migrates: prd.md → spec.md, plan.md → plan.md

# 3. Create constitution
v.speckit constitution
# Generates from CLAUDE.md principles

# 4. Continue with Spec-Kit
/speckit.tasks
/speckit.implement
```

### Workflow 4: Hybrid Mode

```bash
# Major feature: Use Spec-Kit
/speckit.specify New payment integration...
/speckit.plan
/speckit.implement

# Quick fix: Use Standalone
v.createprd "Fix email validation bug"
v.next
v.do

# Keep synced
v.speckit sync
```

## Decision Tree

```
Start new work
    ↓
Is .specify/ present?
    ├─ Yes → Spec-Kit Mode
    │   ├─ Major feature? → /speckit.specify → /speckit.plan → /speckit.implement
    │   └─ Quick task? → v.next → v.do (adapts to read tasks.md)
    │
    └─ No → Standalone Mode
        ├─ Want governance? → v.speckit init (upgrade to Spec-Kit)
        └─ Stay lightweight → v.createprd → v.initproject → v.do
```

## When to Use Which Mode

### ✅ Use Standalone When...
- Solo developer
- Personal project
- Rapid prototyping
- Simple utility
- Learning/experimenting

### ✅ Use Spec-Kit When...
- Team project
- Client work
- Complex feature
- Governance required
- Long-term maintenance

### ✅ Use Hybrid When...
- Growing project
- Mix of quick fixes + major features
- Transitioning practices
- Want flexibility + structure

## Key Principles (Both Modes)

1. **Specs Drive Code** — Not the other way around
2. **Test-First** — Write tests before implementation
3. **Clear Acceptance** — Explicit "done" criteria
4. **Small Files** — 600 line hard cap
5. **Slim Memory** — Archive old work
6. **One Active Task** — Single focus in resume.md
7. **Evidence-Based** — Document completion proof

## Help & Documentation

| Topic | File |
|-------|------|
| Framework overview | [README.md](./README.md) |
| Operating manual | [CLAUDE.md](./CLAUDE.md) |
| Integration guide | [SPECKIT_INTEGRATION.md](./SPECKIT_INTEGRATION.md) |
| Unified principles | [PRINCIPLES.md](./PRINCIPLES.md) |
| Spec-Kit methodology | [.speckit/spec-driven.md](./.speckit/spec-driven.md) |
| Spec-Kit overview | [.speckit/README.md](./.speckit/README.md) |

## Troubleshooting

### "I don't know which mode I'm in"
```bash
v.speckit status
```

### "My commands aren't finding the right files"
Check mode detection—commands auto-adapt based on `.specify/` presence.

### "I want to switch modes"
- Standalone → Spec-Kit: `v.speckit init`
- Spec-Kit → Standalone: `v.speckit sync --to-standalone`

### "File size exceeded 600 lines"
```bash
v.shrink  # Plans and executes split
```

### "Lost track of what I was doing"
```bash
cat _ai/memory/resume.md  # Active operation
v.next                     # Next task
```

### "Need to sync between modes"
```bash
v.speckit sync  # Bidirectional sync
```

---

**Pro Tip**: Most `v.*` commands auto-detect mode and adapt. When in doubt, just run them—they'll find the right files.
