# Vibe-Coding Framework — CLAUDE.md
**Version**: 2.6.0 | **Last Updated**: 2025-11-05
**Owner**: VibeLogic.app — Confidential / Proprietary

Purpose: unified operating manual for AI coding agents working in this repo.
Keep execution predictable, files slim, and context durable across sessions.

Note: we are using default Claude Code syntax and folder structure, so when we refer to a command like
`v.createprd`, if you don't know what it is, look for the file .claude/commands/v.createprd.md
Apply this rule for all the other similar commands

## Project-Specific Rules

**IMPORTANT**: If `PROJECTRULES.md` exists in the project root, read it FIRST before any work.
- Contains project-specific workflows, constraints, and context
- Overrides general framework rules when conflicts exist
- Required for framework development (this repo) and some specialized projects

## Critical Path Guidelines

### Use Relative Paths
- **ALWAYS** use relative paths from current project directory
- **NEVER** use absolute paths starting with `/`
- Use `tmp/` (not `/tmp/`) for temporary files in project root
- Example: `_ai/memory/plan.md` not `/Users/x/project/_ai/memory/plan.md`

### Documentation Principles
1. **Reuse before creating** — Check if documentation already exists
2. **Update, don't duplicate** — Improve existing docs instead of creating variants
3. **Keep it minimal** — Avoid proliferation of similar documents
4. **Archive old content** — Move outdated docs to `_ai/archive/`
5. **Consolidate related topics** — One authoritative doc per topic
6. **Use templates** — Start from `_ai/memory/template/` for memory files

**Bad**: Creating MIGRATION_V2.md, MIGRATION_V3.md, HOW_TO_MIGRATE.md
**Good**: Updating single MIGRATION_GUIDE.md with versioned sections

### Document Placement Rules

When creating new documentation files:

**Framework/Methodology Documentation** → `_ai/`
- Documents about the AI framework itself
- Methodology guides, principles, workflow documentation
- Examples: `_ai/GIT_WORKFLOW.md`, `_ai/PRINCIPLES.md`, `_ai/_ai/FRAMEWORK_IMPROVEMENTS.md`

**Project-Specific Documentation** → `_ai/memory/docs/`
- Technical documentation for the coding project using this framework
- API docs, architecture decisions, project-specific guides
- Examples: `_ai/memory/docs/api-design.md`, `_ai/memory/docs/database-schema.md`

**User-Specified Location** → Follow user instruction
- If user specifies a path, use that exact path
- Example: User says "put it in docs/" → use `docs/`

**Default if uncertain**: Ask user or use `_ai/` for framework-related, `_ai/memory/docs/` for project-related.

### Document Versioning (MANDATORY)

Every documentation file MUST have version header following semantic versioning:

**Format**:
```markdown
# Document Title
**Version**: X.Y.Z | **Last Updated**: YYYY-MM-DD

[Content...]
```

**Versioning Rules** (same as code):
- **X.0.0** (Major) — Breaking changes, complete restructure, incompatible updates
- **0.Y.0** (Minor) — New sections, significant additions, backwards-compatible
- **0.0.Z** (Patch) — Fixes, clarifications, minor updates, typos

**Examples**:
```markdown
# CLAUDE.md
**Version**: 2.3.1 | **Last Updated**: 2025-11-04

# _ai/README.md
**Version**: 1.2.0 | **Last Updated**: 2025-11-04

# v.checkpoint.md
**Version**: 2.0.0 | **Last Updated**: 2025-11-04
```

**When to Update Version**:
- **Patch (0.0.+1)** — Typo fixes, clarifications, formatting
- **Minor (0.+1.0)** — New section, new examples, expanded content
- **Major (+1.0.0)** — Breaking changes, complete rewrite, paradigm shift

**Update Header Every Time**:
- Increment version number appropriately
- Update last modified date to current date (YYYY-MM-DD)
- Add brief change note in document if major/minor

**Enforcement**:
- Agent MUST update version header when modifying any doc
- Agent MUST check current version before updating
- If no version header exists, add one starting at 1.0.0

## Spec-Kit Compatibility Mode

This framework is compatible with the [Spec-Kit](./speckit/) specification-driven development toolkit.
Our lightweight "vibe-coding" approach can operate standalone OR integrate with Spec-Kit's structured process:

### Standalone Mode (Default)
- Use `v.*` commands for lightweight, iterative development
- Memory in `_ai/memory/` with plan/progress/resume pattern
- Optimized for smaller projects and rapid iteration

### Spec-Kit Integration Mode
- When `.specify/.enabled` file exists, we follow Spec-Kit conventions
- Constitution-driven development (`.specify/memory/constitution.md`)
- Feature-branch workflow with numbered specs (`.specify/specs/NNN-feature-name/`)
- Compatible with `/speckit.*` commands (constitution, specify, plan, tasks, implement)
- Our `v.*` commands map to Spec-Kit phases where appropriate

**Key Compatibility Principle**: Specifications should be executable and drive implementation.
Whether using lightweight PRD or full Spec-Kit specs, treat specifications as source of truth.

## Session Boot Order
1. CLAUDE.md                — Framework rules (read first for mode detection)
2. Constitution (project-specific rules):
   - IF .specify/ exists: `.specify/memory/constitution.md`
   - ELSE: `_ai/memory/constitution.md` (if exists)
3. **Mode Detection** (check in this order):
   - IF `.specify/.enabled` exists → **Spec-Kit mode**
   - ELSE IF `_ai/features/active.md` exists → **Feature-Based Standalone**
   - ELSE → **Simple Standalone**
4. Load files based on detected mode:
   
   **Spec-Kit Mode**:
   - .specify/specs/NNN-*/spec.md     — active feature spec
   - .specify/specs/NNN-*/plan.md     — implementation plan
   - .specify/specs/NNN-*/tasks.md    — task breakdown
   
   **Feature-Based Standalone**:
   - _ai/features/active.md           — current active feature (e.g., "F-003")
   - _ai/features/F-NNN/spec.md       — feature requirements
   - _ai/features/F-NNN/plan.md       — feature implementation plan
   - _ai/features/F-NNN/tasks.md      — feature task breakdown
   - _ai/features/F-NNN/progress.md   — feature-specific progress
   - _ai/memory/progress.md           — global session history
   
   **Simple Standalone**:
   - _ai/memory/progress.md   — current state and next steps
   - _ai/memory/resume.md     — single active operation (resume if present)
   - _ai/memory/plan.md       — phases → tasks → steps (IDs)
   - _ai/memory/prd.md        — lightweight requirements (if exists)
   - _ai/memory/techenv.md    — toolchain and commands

## Initialization Rules
Follow this decision tree in order:

### Mode Detection
1. IF `.specify/.enabled` file exists → **Spec-Kit Integration Mode**
2. ELSE → **Standalone Mode** (lightweight)

**Mode Management**: Use `v.mode` command to enable/disable Spec-Kit mode
- `v.mode status` — Show current mode
- `v.mode enable speckit` — Create `.specify/.enabled` marker
- `v.mode disable speckit` — Rename to `.specify/.disabled`

### Spec-Kit Integration Mode
When `.specify/` exists, follow Spec-Kit workflow:
1. Check for `.specify/memory/constitution.md` — if missing, suggest `/speckit.constitution`
2. Use feature-branch pattern: specs in `.specify/specs/NNN-feature-name/`
3. Follow spec → plan → tasks → implement workflow
4. Our `v.*` commands adapt to read from `.specify/` paths

### Standalone Mode
1. IF _ai/memory/legacy/ exists → run **v.initmemory** FIRST to import legacy state
2. THEN IF _ai/memory/plan.md is missing or empty:
   - IF _ai/memory/prd.md exists → convert prd.md into structured plan (PH/T/S)
   - ELSE → run **v.initproject** to bootstrap all memory files
3. Never overwrite an existing plan.md without explicit instruction.

## Command Naming Convention
All custom project tools use the `v.` prefix to avoid conflicts with built-ins.
When in Spec-Kit mode, `/speckit.*` commands are also available for structured workflows.

**Note**: Command definitions are in `.claude/commands/v.*.md`. Documentation is in `_ai/*.md`.

## Framework vs Project Rules

**Root files** (required by AI coding agents):
- **CLAUDE.md** (this file) — Framework rules: how to use commands, manage sessions, file structure.
- **AGENTS.md** — Agent notes: reference to CLAUDE.md.
- **GEMINI.md** — Gemini notes: reference to CLAUDE.md.

**_ai/ directory** (framework files):
- **_ai/*.md** — Documentation (README, QUICKREF, PRINCIPLES, etc.).
- **_ai/memory/** — Project working files.
- **_ai/features/** — Multi-feature tracking (optional).
- **_ai/scripts/** — Helper scripts (optional).

**constitution.md** (project-specific rules):
- Standalone: `_ai/memory/constitution.md`
- Spec-Kit: `.specify/memory/constitution.md`
- Contains: coding standards, architecture patterns, testing requirements, etc.

**Separation of Concerns**:
- CLAUDE.md = framework behavior (same for all projects)
- _ai/ = documentation and working files (copy to any project)
- constitution.md = project specifics (unique to this project)

Create constitution.md using `v.constitution` command. Template at `_ai/memory/template/constitution.md`.

## Core Framework Behavior Rules
- Execute tasks strictly in plan order (PH → T → S). One active step at a time.
- **Before implementing a step:** think, then write a brief TODO checklist under that Task ID in `progress.md` (what you plan to do, in which files), and update `resume.md` with intent/scope/micro-steps.
- **After implementation:** update `progress.md` with findings, decisions, caveats, and verification evidence so `v.memorize` can archive useful knowledge later.
- Hard cap 600 lines per source file. If exceeded, create a refactor task and split.
- Prefer clarity over cleverness; minimize dependencies; short, single-responsibility functions.
- Tests first when feasible; fast, hermetic by default; integration/e2e are opt-in.
- Never commit secrets; use environment variables and .env.example files.
- Don’t reorder/skip tasks without updating plan.md and progress.md.

### Default Behavior When User Skips Process

**CRITICAL**: Even when user directly requests a bug fix or feature WITHOUT explicitly mentioning the workflow, ALWAYS follow this protocol:

1. **Assess Current State**:
   - Check if plan.md exists and contains the requested work
   - If not, create/update plan.md with proper PH/T/S structure
   - Never implement directly without a task in plan.md

2. **Git Safety Check** (see Git Workflow Protocol below):
   - Check `git status` for uncommitted changes
   - If substantial work in progress → suggest `v.checkpoint` first
   - Only proceed after clean state or explicit user override

3. **Structured Approach**:
   ```
   User: "Fix the login bug"
   → You: Check plan.md → Add T-XXX if missing → Run v.next → v.do
   
   User: "Add dark mode feature"
   → You: Check if in plan → If not: create task in plan.md → v.next → v.do
   
   User: "The button doesn't work"
   → You: Diagnose → Add debug/fix task to plan.md → v.next → v.do
   ```

4. **Response Pattern**:
   - Acknowledge request
   - State what you'll add to plan (if missing)
   - Execute proper workflow (v.next → v.do)
   - Update progress.md with findings

**Exception**: Only skip workflow for:
- Read-only diagnostic requests ("What's in this file?")
- Metadata queries ("List all tasks")
- Workflow commands themselves (v.checkpoint, v.memorize, etc.)


### Template Usage
- All memory files have templates in `_ai/memory/template/`
- When initializing missing files, copy from template and customize
- Templates include: `plan.md`, `progress.md`, `resume.md`, `constitution.md`, `techenv.md`
- This allows copying `_ai/` to new projects without overwriting existing memory

## Git Workflow Protocol

**Philosophy**: Commit early, commit often. Never lose working code.

### Mandatory Git Checkpoints

1. **Before Major Work**:
   - Starting a new Phase (PH-XX) → Commit with message: `checkpoint: starting PH-XX - <description>`
   - Starting a Task with >3 steps → Commit: `checkpoint: before T-XXX - <task-title>`

2. **After Each Completed Task**:
   - When marking Task as done → Run `v.checkpoint` (auto-commits)
   - Commit message format: `feat(T-XXX): <task-title>` or `fix(T-XXX): <description>`

3. **Safety Checks Before Implementation**:
   ```bash
   # ALWAYS run before v.do or v.implement:
   git status --short
   
   # If changes exist:
   - ≤10 lines uncommitted → Safe to proceed
   - >10 lines uncommitted → STOP: suggest v.checkpoint first
   - Untracked files → List and ask if they should be committed
   ```

4. **Working Branch Strategy** (Recommended):
   - For experimental/risky work: suggest creating feature branch
   - Pattern: `git checkout -b feature/T-XXX-short-name`
   - Merge back when tests pass and user satisfied
   - User decides: branch vs direct commits on main

### Auto-Checkpoint Triggers

`v.checkpoint` command automatically runs when:
- Completing a Task (user runs v.checkpoint)
- Every 3 completed steps in a long task (optional, user preference)
- Before running `v.shrink` (file refactoring)
- User explicitly requests it

### Commit Message Convention

Follow Conventional Commits format:
```
feat(T-XXX): add user authentication
fix(T-XXX): resolve login redirect bug
refactor(T-XXX): split UserService into modules
docs(T-XXX): update API documentation
test(T-XXX): add integration tests for payment flow
checkpoint: before T-XXX - about to refactor database layer
```

### Recovery Strategy

If implementation goes wrong:
1. `git status` to see changes
2. `git diff` to review what broke
3. Options:
   - `git restore <file>` — discard changes to specific file
   - `git restore .` — discard all changes (nuclear option)
   - `git stash` — save for later without committing
4. Document in progress.md what went wrong
5. Adjust plan.md if approach needs changing
## Memory Protocol
- **progress.md**: COMPLETED work, session summaries, decisions (past tense) — updated after each step.
- **resume.md**: IN-FLIGHT work only (present tense) — single active entry; clear when done.
- After each step: update progress.md (state, next, decisions) and tick plan.md.
- If interrupted: write/keep a single active entry in resume.md; resume it next session.
- `v.memorize` archives progress **by Task ID** into `_ai/memory/archive/T-xxx.md`.
- **archive/index.md**: Optional but recommended for repos with >10 tasks; auto-created by v.memorize if 5+ archive files exist.
- When plan/progress become large, move details to `_ai/memory/archive/<ID>.md` and keep pointers.
- Checkpoints run `v.memorize` to keep memory slim and current.

### Template Usage
- All memory files have templates in `_ai/memory/template/`
- When initializing missing files, copy from template and customize
- Templates include: `plan.md`, `progress.md`, `resume.md`, `constitution.md`, `techenv.md`
- This allows copying `_ai/` to new projects without overwriting existing memory

## Git Workflow Protocol

**Philosophy**: Commit early, commit often. Never lose working code.

### Mandatory Git Checkpoints

1. **Before Major Work**:
   - Starting a new Phase (PH-XX) → Commit with message: `checkpoint: starting PH-XX - <description>`
   - Starting a Task with >3 steps → Commit: `checkpoint: before T-XXX - <task-title>`

2. **After Each Completed Task**:
   - When marking Task as done → Run `v.checkpoint` (auto-commits)
   - Commit message format: `feat(T-XXX): <task-title>` or `fix(T-XXX): <description>`

3. **Safety Checks Before Implementation**:
   ```bash
   # ALWAYS run before v.do or v.implement:
   git status --short
   
   # If changes exist:
   - ≤10 lines uncommitted → Safe to proceed
   - >10 lines uncommitted → STOP: suggest v.checkpoint first
   - Untracked files → List and ask if they should be committed
   ```

4. **Working Branch Strategy** (Recommended):
   - For experimental/risky work: suggest creating feature branch
   - Pattern: `git checkout -b feature/T-XXX-short-name`
   - Merge back when tests pass and user satisfied
   - User decides: branch vs direct commits on main

### Auto-Checkpoint Triggers

`v.checkpoint` command automatically runs when:
- Completing a Task (user runs v.checkpoint)
- Every 3 completed steps in a long task (optional, user preference)
- Before running `v.shrink` (file refactoring)
- User explicitly requests it

### Commit Message Convention

Follow Conventional Commits format:
```
feat(T-XXX): add user authentication
fix(T-XXX): resolve login redirect bug
refactor(T-XXX): split UserService into modules
docs(T-XXX): update API documentation
test(T-XXX): add integration tests for payment flow
checkpoint: before T-XXX - about to refactor database layer
```

### Recovery Strategy

If implementation goes wrong:
1. `git status` to see changes
2. `git diff` to review what broke
3. Options:
   - `git restore <file>` — discard changes to specific file
   - `git restore .` — discard all changes (nuclear option)
   - `git stash` — save for later without committing
4. Document in progress.md what went wrong
5. Adjust plan.md if approach needs changing

## Size & Quality Guardrails
- **File size policy**:
  - 200–400 lines: ideal range (no action needed)
  - 500 lines: warning threshold (log in next checkpoint)
  - 600 lines: hard limit (MUST split before merging)
- Safe logging (no sensitive data); defensive input validation.
- Public APIs documented briefly; internal code covered by tests.

## Acceptance Protocol
A step is "done" when all of these apply:
- Acceptance criteria from plan.md are met (verify manually)
- IF tests exist → tests pass
- IF linter/formatter configured → lint/format pass
- progress.md and plan.md updated
Record a short "✅ Done: <step-id> — evidence."
If blocked, add "❌ <step-id> — blocker + next micro-step" and keep it in resume.md.

## Command Table

### Project Initialization Commands

- **v.onboard** — Reverse engineer existing project and generate all AI memory files
- **v.initproject** — Bootstrap new project from scratch (legacy)
- **v.initmemory** — Import legacy memory files from _ai/memory/legacy/

### Spec-Kit Equivalent Commands (PRIMARY)

These commands mirror the Spec-Kit workflow and work in both modes:

- **v.constitution** — Create/update project governing principles
- **v.specify** — Define requirements and user stories (focus on WHAT/WHY)
- **v.plan** — Create technical implementation plan with tech stack (focus on HOW)
- **v.tasks** — Generate actionable task breakdown with dependencies
- **v.implement** — Execute all tasks automatically in correct order

**Workflow**: 
- **New project**: `v.constitution` → `v.specify` → `v.plan` → `v.tasks` → `v.implement`
- **Existing project**: `v.onboard` → review → `v.next` → `v.do`

### Session Management Commands

- **v.next** — Read plan/tasks → output the next executable step
- **v.do** — Start/continue the next step; log in resume.md; update progress
- **v.resume** — Reload and finish the single active operation
- **v.checkpoint** — Run shrink + testsync + build; memorize; commit milestone

### Quality & Maintenance Commands

- **v.review** — Lint/format, security hygiene, readability; re-run tests
- **v.shrink** — Enforce size caps; plan and execute file splits
- **v.testsync** — Make tests compile with current APIs; list behavioral failures

### Memory Management Commands

- **v.memorize** — Sync progress/changelog; rotate archives by Task ID; maintain index
- **v.archive** — Move bulky historical details to _ai/memory/archive/
- **v.initmemory** — Import legacy memory files from _ai/memory/legacy/

### Feature Management Commands

- **v.feature** — Manage multiple features in Standalone mode
  - `new <name>` — Create new feature
  - `list` — List all features with status
  - `switch <id>` — Switch to different feature
  - `pause [id]` — Pause current feature
  - `resume <id>` — Resume paused feature
  - `status` — Show current active feature

### Utility Commands

- **v.help** — Interactive help and workflow guidance (context-aware)
- **v.whatif** — Critically evaluate ideas; if accepted, add to plan
- **v.syncdocs** — Update README/QUICKSTART snippets from current rules
- **v.docdiagrams** — Generate/update architecture diagrams (optional)

### Integration Commands

- **v.speckit** — Spec-Kit bridge for mode management
  - `v.speckit init` — Convert to Spec-Kit mode
  - `v.speckit status` — Show active mode
  - `v.speckit constitution` — Generate constitution
  - `v.speckit sync` — Bidirectional sync

### Legacy Aliases (Backward Compatibility)

- **v.createprd** → `v.specify` (aliased)
- **v.initproject** — Still works for bootstrapping memory structure

### Quality Assurance Commands

- **v.clarify** — Structured clarification of underspecified requirements
- **v.analyze** — Cross-artifact consistency and coverage analysis
- **v.checklist** — Generate custom quality validation checklists

### Spec-Kit Native Commands (when `.specify/` exists)

When in Spec-Kit mode, these commands are also available:

- **/speckit.constitution** — Same as v.constitution
- **/speckit.specify** — Same as v.specify
- **/speckit.plan** — Same as v.plan
- **/speckit.tasks** — Same as v.tasks
- **/speckit.implement** — Same as v.implement
- **/speckit.clarify** — Same as v.clarify
- **/speckit.analyze** — Same as v.analyze
- **/speckit.checklist** — Same as v.checklist

### Command Behavior by Mode

Commands intelligently adapt based on directory structure:

| Command | Simple Standalone | Feature-Based Standalone | Spec-Kit |
|---------|------------------|-------------------------|----------|
| `v.constitution` | Creates `_ai/memory/constitution.md` | Same | Creates `.specify/memory/constitution.md` |
| `v.specify` | Creates/updates `prd.md` | Updates `F-NNN/spec.md` | Creates `.specify/specs/NNN-*/spec.md` |
| `v.plan` | Creates/updates `plan.md` | Creates `F-NNN/plan.md` | Creates `.specify/specs/NNN-*/plan.md` |
| `v.tasks` | Enhances `plan.md` | Creates `F-NNN/tasks.md` | Creates `.specify/specs/NNN-*/tasks.md` |
| `v.next` | Reads `plan.md` | Reads `F-NNN/tasks.md` | Reads `.specify/specs/NNN-*/tasks.md` |
| `v.do` | Updates `progress.md` | Updates `F-NNN/progress.md` + global | Checks constitution + updates spec progress |
| `v.feature` | N/A | Manages features in `_ai/features/` | Managed by Spec-Kit (not needed) |

## AI Agent Boot Checklist

1. Read files in the Session Boot Order above.

2. **Mode Detection**:
   - Check if `.specify/` exists → Spec-Kit mode
   - Else → Standalone mode

3. **Initialization** (follow in order):
   
   **IF legacy migration needed**:
   - IF `_ai/memory/legacy/` exists → run **v.initmemory** first
   
   **IF starting new work**:
   - No spec? → Run **v.constitution** (optional) → **v.specify** → **v.plan** → **v.tasks**
   - Spec exists but no plan? → Run **v.plan** → **v.tasks**
   - Plan exists but no tasks? → Run **v.tasks**
   
   **Legacy initialization** (backward compatible):
   - IF prd.md exists but no plan → run **v.initproject** (converts to plan.md)

4. **Resume or Start**:
   - IF `_ai/memory/resume.md` has an active entry → run **v.resume**
   - ELSE → run **v.next** → write TODO in progress.md → **v.do**
   - OR for full automation → run **v.implement** (executes all tasks)

5. **At stabilizing points** → run **v.checkpoint**

## Recommended Workflows

### Existing Project (Onboard & Continue)
```
v.onboard → review files → v.next → v.do → v.checkpoint
```

### Existing Project (Reverse Engineer with QA)
```
v.onboard --deep → v.analyze → v.clarify → v.next → v.do
```

### New Project (Modern, with QA)
```
v.constitution → v.specify → v.clarify → v.checklist spec → 
v.plan → v.tasks → v.analyze → v.implement
```

### New Project (Quick)
```
v.specify → v.plan → v.tasks → v.implement
```

### New Project (Manual Control)
```
v.specify → v.plan → v.next → v.do → v.checkpoint
```

### Existing Project (Resume Work)
```
v.resume (if interrupted) or v.next → v.do
```

### Legacy Memory Files (Upgrade)
```
v.initmemory → v.specify → v.plan → v.tasks
```

### Quality-Focused Workflow
```
v.specify → v.checklist spec → (fix issues) →
v.clarify → v.plan → v.checklist plan → (fix issues) →
v.tasks → v.analyze → (fix issues) → v.implement
```

### Fast Vibe Coding (Rapid Prototyping)
```
v.specify → v.plan → v.tasks → v.doall
```
**Purpose**: Maximum speed, autonomous execution, auto-commits  
**When to use**: Prototyping, exploration, time-constrained sprints  
**Tradeoffs**: Less manual review, auto-debugging, speed over perfection

### Fast Vibe Coding (Ultra-Quick)
```
v.specify "Build X with Y tech" → v.doall
```
**Purpose**: Minimal setup, immediate execution  
**Note**: `v.doall` will auto-generate plan and tasks if missing

## Error Recovery Protocol
- **Step fails**: Update resume.md with blocker → run **v.whatif** for alternatives → adjust plan.md
- **Tests fail after implementation**: Categorize as test bug vs behavioral issue → fix or document
- **Need rollback**: Git revert → document reason in progress.md → adjust plan.md
- **Blocked on external dependency**: Mark in resume.md → move to next unblocked task → revisit later

## Design Philosophy Alignment

### Spec-Kit Principles We Adopt
1. **Specifications as Source of Truth** — Specs drive implementation, not the other way around
2. **Constitution-Driven Development** — Immutable principles guide all decisions
3. **Test-First Approach** — Tests written before implementation (where feasible)

**Note**: Full principles documentation in `_ai/PRINCIPLES.md`.
4. **Library-First Architecture** — Features as standalone, testable libraries
5. **Intent-Driven Development** — Focus on "what" and "why" before "how"

### Our Lightweight Extensions
1. **Vibe-Coding** — Flexible, iterative refinement for smaller projects
2. **Minimal File Count** — Keep core files under 600 lines, split when needed
3. **Memory Archiving** — Task-indexed archives prevent bloat
4. **Resume Protocol** — Single active operation for interruption recovery
5. **Standalone Capable** — Full workflow without `.specify/` structure

### Best of Both Worlds
- **Small Projects**: Use `v.*` commands with `_ai/memory/` for lightweight iteration
- **Large Projects**: Use `/speckit.*` with `.specify/` for structured governance
- **Hybrid**: Start with `v.*`, migrate to Spec-Kit as complexity grows
- **Constitution Optional**: But recommended for multi-developer or long-term projects

Version: 2.3 (Spec-Kit compatibility + dual-mode operation)
© VibeLogic.app — Confidential / Proprietary — Do Not Redistribute
