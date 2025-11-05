# Vibe-Coding Framework
**Version**: 1.1.0 | **Last Updated**: 2025-11-04

**Owner**: VibeLogic.app — Confidential / Proprietary

A minimal, plan-driven methodology for building software with AI coding agents.
It keeps context durable (`_ai/memory/`), code lean, and progress explicit.

## 🚀 Quick Start

**New to the framework?** Follow this order:

1. Read [CLAUDE.md](../CLAUDE.md) — Framework operating manual (required reading)
2. Read this file (you're here!) — Overview and structure
3. Check [QUICKSTART.md](./QUICKSTART.md) — Get started in 5-10 minutes
4. Use [QUICKREF.md](./QUICKREF.md) — Command cheat sheet (bookmark this)

**Optional**: [Spec-Kit CLI](https://github.com/github/spec-kit) for structured spec-driven development:
```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

## 📁 Directory Structure

```
project-root/
├── CLAUDE.md              # Framework operating manual (required)
├── AGENTS.md              # Agent operating notes
├── GEMINI.md              # Gemini-specific instructions
├── CHANGELOG.md           # Version history
│
├── _ai/                   # Framework directory (copy to projects)
│   ├── README.md          # This file - overview and structure
│   ├── QUICKSTART.md      # Getting started guide
│   ├── QUICKREF.md        # Command quick reference
│   ├── PRINCIPLES.md      # Development principles
│   ├── GIT_WORKFLOW.md    # Git workflow integration
│   ├── FRAMEWORK_IMPROVEMENTS.md  # Framework improvements log
│   │
│   ├── memory/            # Project working files
│   │   ├── template/      # Templates for new projects
│   │   │   ├── plan.md
│   │   │   ├── progress.md
│   │   │   ├── resume.md
│   │   │   ├── constitution.md
│   │   │   └── techenv.md
│   │   │
│   │   └── docs/          # Project-specific documentation (optional)
│   │       └── (API docs, architecture, etc.)
│   │
│   ├── features/          # Multi-feature tracking (optional)
│   │   ├── README.md
│   │   ├── active.md
│   │   └── F-NNN-name/
│   │       ├── spec.md
│   │       ├── plan.md
│   │       ├── tasks.md
│   │       └── progress.md
│   │
│   └── scripts/           # Helper scripts (optional)
│
├── .claude/
│   └── commands/          # v.* command definitions
│
├── .speckit/              # Spec-Kit mode (optional)
│
├── archive/               # Historical/migration docs (don't copy to projects)
│   ├── MIGRATION_GUIDE.md
│   ├── SPECKIT_INTEGRATION.md
│   └── ...
│
└── tmp/                   # Temporary files (project-relative)
```

## 📦 Setup New Project

Copy framework to any project:

```bash
# Minimal setup (just copy these)
cp -r path/to/framework/_ai .
cp -r path/to/framework/.claude .
cp path/to/framework/{CLAUDE.md,AGENTS.md,GEMINI.md,CHANGELOG.md} .

# DON'T copy archive/ (historical docs only)

# Initialize project memory from templates
cp _ai/memory/template/plan.md _ai/memory/
cp _ai/memory/template/progress.md _ai/memory/
cp _ai/memory/template/resume.md _ai/memory/
cp _ai/memory/template/techenv.md _ai/memory/

# Optional: project-specific docs folder
mkdir -p _ai/memory/docs

# Optional: Spec-Kit mode
cp -r path/to/framework/.speckit .
```

## 📖 Core Files

### Root Files (Required by AI coding agents)
- **CLAUDE.md** — Framework operating rules (how to use the framework).
- **AGENTS.md** — Agent operating notes (reference to CLAUDE.md).
- **GEMINI.md** — Gemini-specific instructions (reference to CLAUDE.md).
- **CHANGELOG.md** — Version history.

### _ai/ Directory (Framework files - copy to projects)

**Essential Documentation**:
- **README.md** — This file, framework overview
- **QUICKSTART.md** — Getting started in 5-10 minutes
- **QUICKREF.md** — Command cheat sheet
- **PRINCIPLES.md** — Development principles
- **GIT_WORKFLOW.md** — Git workflow integration
- **FRAMEWORK_IMPROVEMENTS.md** — Framework improvements log

### _ai/memory/ (Project working files)
- **_ai/memory/constitution.md** — Project-specific rules (coding standards, architecture, testing).
- **_ai/memory/prd.md** — Product/tech spec; source for the first plan.
- **_ai/memory/plan.md** — Phases → Tasks → Steps (IDs, acceptance).
- **_ai/memory/progress.md** — Concise journal (state, next, decisions, blockers).
- **_ai/memory/resume.md** — Single active operation for continuity.
- **_ai/memory/archive/** — Long-form task/decision details by ID.
- **_ai/memory/techenv.md** — Toolchain and commands (from template).

### _ai/memory/template/ (Templates for new projects)
Templates to copy when starting a new project:
- **plan.md** — Task planning with PH/T/S structure
- **progress.md** — Progress tracking pattern
- **resume.md** — Resume interrupted work
- **constitution.md** — Project-specific rules
- **techenv.md** — Toolchain and environment

### _ai/memory/docs/ (Project documentation - optional)
For project-specific technical documentation:
- API design documents
- Architecture decision records
- Database schemas
- Integration guides

### _ai/features/ (Multi-feature tracking - optional)
For projects with multiple concurrent features:
- **README.md** — Feature structure explanation
- **active.md** — Current active feature
- **F-NNN-name/** — Feature directories

### .speckit/ (Spec-Kit mode - optional)
Full Spec-Kit framework for structured specification-driven development.

### archive/ (Historical docs - DON'T copy to projects)
Historical and migration documentation. Reference only.

**Need help?** Run `v.help` for interactive guidance.

## 🎯 Common Scenarios

### "I'm new to this framework"
1. Read [CLAUDE.md](../CLAUDE.md) (10 min) — Framework rules
2. Read [QUICKSTART.md](./QUICKSTART.md) (5 min) — Get started
3. Keep [QUICKREF.md](./QUICKREF.md) open — Command reference

### "I want to start a new project"
1. Copy `_ai/`, `.claude/`, and root files to your project
2. Copy templates from `_ai/memory/template/` to `_ai/memory/`
3. Run `v.constitution` (optional) then `v.specify` → `v.plan` → `v.implement`

### "I need to know what command to use"
→ Check [QUICKREF.md](./QUICKREF.md)

### "I want to understand the methodology"
→ Read [PRINCIPLES.md](./PRINCIPLES.md)

### "I need git workflow help"
→ Read [GIT_WORKFLOW.md](./GIT_WORKFLOW.md)

## Workflow (High-Level)

### Modern Workflow (Recommended)
1. **v.constitution** — Define project principles (optional but recommended)
2. **v.specify** — Define requirements (what you're building and why)
3. **v.clarify** — Clarify ambiguities (optional but recommended)
4. **v.plan** — Create implementation plan (tech stack and how to build)
5. **v.tasks** — Generate detailed task breakdown with dependencies
6. **v.analyze** — Validate consistency and coverage (optional but recommended)
7. **v.implement** — Execute all tasks automatically, or use `v.next` → `v.do` for manual control
8. **v.checkpoint** — Stabilize (runs shrink/testsync/build, then memorize)

### Legacy Workflow (Still Supported)
1. Draft **prd.md** → `v.createprd` (aliased to `v.specify`)
2. Initialize docs → `v.initproject` (bootstraps memory structure)
3. Ask what to do → `v.next` → execute → `v.do`
4. Stabilize → `v.checkpoint`

## Migrating Legacy Projects

If you are upgrading an older project that used a different memory or documentation structure:

1. Copy all relevant legacy files into `_ai/memory/legacy/`.  
   Include any of:
   - Old progress logs, memoryrules, planning files, changelogs, etc.
2. Run:
   ```bash
   v.initmemory
   ```
   This parses and merges the legacy information into the new architecture:
   - Plans → `_ai/memory/plan.md`
   - Progress/state → `_ai/memory/progress.md`
   - Incomplete work → `_ai/memory/resume.md`
   - Long details → `_ai/memory/archive/`
3. After import:
   - Run `v.memorize` to summarize the new state
   - Run `v.checkpoint` to validate and commit
   - If plan structure changed significantly, run `v.initproject` to regenerate linked docs

Your legacy files remain untouched in `_ai/memory/legacy/` for provenance.

## Command Suite

### Primary Workflow Commands (Spec-Kit Compatible)

**Main workflow** (mirrors Spec-Kit):
- **v.constitution** — Create project governing principles
- **v.specify** — Define requirements (WHAT/WHY)
- **v.plan** — Create implementation plan (HOW/tech stack)
- **v.tasks** — Generate task breakdown
- **v.implement** — Execute all tasks automatically

**Session management**:
- **v.next** — Show next task
- **v.do** — Execute current task
- **v.resume** — Resume interrupted work
- **v.checkpoint** — Stabilize and commit

**Quality & validation**:
- **v.clarify** — Clarify underspecified requirements
- **v.analyze** — Cross-artifact consistency analysis
- **v.checklist** — Generate quality validation checklists
- **v.review** — Code review and validation
- **v.shrink** — Enforce file size limits
- **v.testsync** — Sync tests with code

**Memory management**:
- **v.memorize** — Archive completed work
- **v.archive** — Manual archiving
- **v.initmemory** — Import legacy data

**Feature management**:
- **v.feature** — Manage multiple features (new/list/switch/pause/resume/status)

**Utility**:
- **v.help** — Interactive help and guidance
- **v.whatif** — Evaluate ideas
- **v.syncdocs** — Update documentation

### Spec-Kit Integration

**v.speckit** — Bridge command for Spec-Kit compatibility:
- `v.speckit init` — Convert to Spec-Kit mode
- `v.speckit status` — Show active mode and paths
- `v.speckit constitution` — Generate constitution from practices
- `v.speckit sync` — Bidirectional sync between modes

When `.specify/` exists, `/speckit.*` commands also become available.  
See [SPECKIT_INTEGRATION.md](./SPECKIT_INTEGRATION.md) for complete guide.

### Legacy Commands (Still Supported)

- **v.createprd** → `v.specify` (aliased)
- **v.initproject** — Bootstrap memory structure

## Quality Guardrails
- Hard cap 600 lines per file; refactor if exceeded.
- Short, clear, dependency-light code; tests and lint before “done.”
- No secrets in code or logs.

© VibeLogic.app — Confidential / Proprietary — Do Not Redistribute
