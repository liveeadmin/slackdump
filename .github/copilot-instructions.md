# GitHub Copilot Instructions — VibeLogic AI Framework

This repository contains the **VibeLogic "v." framework**, a plan-driven methodology for building software with AI coding agents.

---

## Framework Overview

**Operating Manual**: Always read `CLAUDE.md` first — it contains the complete framework rules and workflows.

**Philosophy**: 
- Plan-driven development with structured memory files
- Task-based workflow (Phases → Tasks → Steps)
- AI-friendly context management
- Works standalone or with Spec-Kit integration

---

## Available Commands

This project provides 29 custom commands via `.claude/commands/`. When suggesting code or workflows, reference these commands as they define the framework's capabilities.

### Project Initialization

- **@.claude/commands/v.onboard.md**  
  Analyze existing codebase to understand architecture, goals, and context, then generate all necessary AI memory files (PRD, plan, tasks, constitution, techenv) to continue development using the vibelogic workflow. Reverse engineer projects built without AI or with different AI tools.

- **@.claude/commands/v.constitution.md**  
  Create or update project governing principles that guide all development decisions. Works in both Standalone and Spec-Kit modes.

- **@.claude/commands/v.initproject.md**  
  Generate minimal, consistent files from `_ai/memory/prd.md` and initialize the project memory structure. (Legacy command, use v.specify → v.plan → v.tasks for new projects)

- **@.claude/commands/v.initmemory.md**  
  Parse files from `_ai/memory/legacy/` and import their content into the standard memory architecture.

### Requirements & Planning

- **@.claude/commands/v.specify.md**  
  Create or refine feature specifications. Focus on **what** you're building and **why**, not the tech stack. Modern replacement for v.createprd.

- **@.claude/commands/v.createprd.md**  
  Legacy alias for v.specify. Create or refine feature specifications focusing on what and why.

- **@.claude/commands/v.clarify.md**  
  Systematically identify and resolve ambiguities, gaps, and underspecified areas in the specification before creating implementation plan. Reduces downstream rework.

- **@.claude/commands/v.plan.md**  
  Generate implementation plan from specification. Define **how** to build it with tech stack and architecture. Partially replaces v.initproject.

- **@.claude/commands/v.tasks.md**  
  Break implementation plan into specific, actionable, ordered tasks with dependencies and acceptance criteria.

### Execution & Workflow

- **@.claude/commands/v.next.md**  
  Read plan and progress, then output the next executable step and its acceptance criteria.

- **@.claude/commands/v.do.md**  
  Start or continue the next step from plan.md. Keep scope tight and log to resume.md. Works in both Standalone and Spec-Kit modes.

- **@.claude/commands/v.resume.md**  
  Open the single entry in `_ai/memory/resume.md` and continue exactly where it left off.

- **@.claude/commands/v.implement.md**  
  Automatically execute all tasks from task list in correct order, respecting dependencies, parallelization, and checkpoints.

- **@.claude/commands/v.doall.md**  
  Execute all tasks automatically without stopping. Autonomous problem-solving with TDD, memory updates, and auto-commits. Fast vibe coding mode.

### Quality Assurance

- **@.claude/commands/v.analyze.md**  
  Validate consistency and coverage across specification, plan, and tasks. Identifies gaps, conflicts, and misalignments before implementation.

- **@.claude/commands/v.checklist.md**  
  Create custom quality checklists that validate requirements completeness, clarity, and consistency. Like "unit tests for English" — ensure specifications meet quality standards before implementation.

- **@.claude/commands/v.review.md**  
  Lightweight pre-commit quality sweep rooted in clarity, safety, and minimalism.

- **@.claude/commands/v.testsync.md**  
  Make test files compile with current APIs; separate behavioral failures for later debugging.

- **@.claude/commands/v.shrink.md**  
  Detect and refactor oversized source files using the 600-line hard cap policy.

### Memory & Documentation

- **@.claude/commands/v.checkpoint.md**  
  Freeze a milestone with validated code and up-to-date memory. Auto-commits with conventional commit messages.

- **@.claude/commands/v.memorize.md**  
  Persist today's work to `progress.md`, keep live files slim, and make history discoverable by **Task ID** (e.g., T-010).

- **@.claude/commands/v.archive.md**  
  Keep progress.md and plan.md slim by offloading long sections to archive.

- **@.claude/commands/v.syncdocs.md**  
  Regenerate README/QUICKSTART snippets to reflect current commands and workflow.

### Feature Management

- **@.claude/commands/v.feature.md**  
  Track and manage multiple features in Standalone mode, similar to Spec-Kit's feature tracking but with lightweight flexibility.

### Mode & Integration

- **@.claude/commands/v.mode.md**  
  Manage framework operating mode (Standalone vs Spec-Kit).

- **@.claude/commands/v.speckit.md**  
  Enable Spec-Kit compatibility mode and bridge between vibe-coding and spec-driven workflows.

### Utilities

- **@.claude/commands/v.help.md**  
  Guide users through the framework with interactive help, command suggestions, and workflow recommendations based on current project state.

- **@.claude/commands/v.whatif.md**  
  Apply critical thinking to decide: proceed, defer, simplify, alternative, or skip.

---

## Memory Structure

The framework uses `_ai/memory/` for project context:

```
_ai/memory/
├── constitution.md    — Project-specific rules and principles
├── prd.md            — Product requirements and features
├── plan.md           — Implementation plan (PH-XX / T-XXX / S-XXX)
├── tasks.md          — Detailed task breakdown (optional, can be in plan.md)
├── techenv.md        — Tech stack, setup, build, test commands
├── progress.md       — Completed work and session history
├── resume.md         — Single in-flight operation (for interruptions)
└── archive/          — Historical task details by Task ID
```

---

## Recommended Workflows

### For Existing Projects
```bash
v.onboard .                    # Reverse engineer and generate memory
# Review generated files
v.next                         # See next task
v.do                           # Start working
v.checkpoint                   # Save progress
```

### For New Projects (Quick)
```bash
v.specify "Build a [feature]"  # Define requirements
v.plan "Tech stack details"    # Create implementation plan
v.tasks                        # Generate task breakdown
v.implement                    # Execute all tasks
```

### For New Projects (Manual Control)
```bash
v.specify "Build a [feature]"
v.plan "Tech stack"
v.tasks
v.next                         # See next step
v.do                           # Execute step
v.checkpoint                   # Save milestone
```

### Quality-Focused Workflow
```bash
v.specify "..."
v.checklist spec               # Validate requirements
v.clarify                      # Resolve ambiguities
v.plan "..."
v.analyze                      # Check consistency
v.implement
```

---

## Key Principles

When suggesting code or workflows:

1. **Plan First**: Always work from structured plans (PH/T/S hierarchy)
2. **Memory-Driven**: Update progress.md and plan.md after each step
3. **Git Safety**: Check git status before major work, checkpoint frequently
4. **Size Limits**: 600-line hard cap per file, split when exceeded
5. **Task Focus**: One task/step at a time, mark complete before moving on
6. **Constitution**: Follow project-specific rules in constitution.md
7. **Relative Paths**: Use relative paths (e.g., `_ai/` not `/_ai/`)

---

## Command Reference Pattern

When suggesting a command, use this format:

```bash
# Command with purpose
v.specify "Build authentication system"
# Creates/updates _ai/memory/prd.md with requirements

v.plan "Node.js + JWT + PostgreSQL"
# Generates _ai/memory/plan.md with structured PH/T/S plan

v.next
# Shows next step from plan.md with acceptance criteria

v.do
# Executes the step and updates progress.md
```

---

## File Reference Pattern

When referencing framework files:

- **Commands**: `@.claude/commands/v.[command].md`
- **Operating Manual**: `@CLAUDE.md`
- **Memory Files**: `@_ai/memory/[file].md`
- **Documentation**: `@_ai/[doc].md`

---

## Integration Notes

This framework is compatible with:
- **Standalone Mode**: Lightweight workflow with `_ai/memory/`
- **Spec-Kit Mode**: Structured workflow with `.specify/`
- **Other AI Tools**: Can import context from Cursor, Copilot, Aider

Use `v.mode status` to check current mode.

---

## Version

**Framework**: v2.5.0  
**Last Updated**: 2025-11-04  
**Owner**: VibeLogic.app

---

## Quick Help

For interactive guidance within the framework:
```bash
v.help              # Interactive menu
v.help setup        # Setup guide
v.help workflow     # Workflow guidance
v.help commands     # Command list
```

For detailed documentation, see:
- `@CLAUDE.md` — Complete operating manual
- `@_ai/QUICKREF.md` — Command quick reference
- `@_ai/README.md` — Framework overview

---

**Note**: When GitHub Copilot suggests code, it should respect the framework's structure, use the available commands, and follow the plan-driven workflow defined in this repository.
