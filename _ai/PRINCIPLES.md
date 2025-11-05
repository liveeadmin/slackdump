# Development Principles — Unified Framework

This document consolidates principles from both our Vibe-Coding framework and Spec-Kit's specification-driven methodology.

## Core Philosophy

### The Power Inversion (from Spec-Kit)

**Traditional Development**: Code is king → Specifications serve code  
**Our Approach**: Specifications are king → Code serves specifications

**What This Means**:
- Specifications are the source of truth, not documentation
- Implementation should be regenerable from specs
- Changes happen at the spec level, then propagate to code
- Debugging means fixing specs, not just code

### Intent-Driven Development

Focus on **what** and **why** before **how**:
- Define requirements in natural language first
- Let AI translate intent into technical decisions
- Iterate on specifications before writing code
- Specifications should be clear enough to generate working code

## Principles We Share with Spec-Kit

### 1. Specifications Drive Implementation

**Spec-Kit**: Specifications are executable—they generate code  
**Vibe-Coding**: PRD/plan defines work; implementation follows strictly  
**Unified**: Whether lightweight PRD or full spec, it's the source of truth

### 2. Test-First Development

**Both**: Write tests before implementation where feasible  
**Both**: Red-Green-Refactor cycle for TDD  
**Both**: Tests define acceptance criteria explicitly

**Implementation**:
- Tests written → User approved → Tests fail → Implement
- Fast, hermetic tests by default
- Integration tests for contracts and inter-service communication

### 3. Constitution/Principles Guide Decisions

**Spec-Kit**: constitution.md with immutable core principles  
**Vibe-Coding**: CLAUDE.md Core Behavior Rules  
**Unified**: Explicit principles prevent ad-hoc decision-making

**Examples**:
- Library-first: Features as standalone, testable modules
- Observability: Text I/O for debuggability
- Simplicity: Start simple, add complexity only when proven necessary
- Security: Never commit secrets, validate inputs

### 4. Clear Acceptance Criteria

**Both**: Every task has explicit "done" conditions  
**Both**: Evidence-based completion (tests pass, requirements met)  
**Both**: No ambiguity about what "complete" means

### 5. File Size Discipline

**Vibe-Coding**: Hard cap 600 lines, split when exceeded  
**Spec-Kit**: Template-based structure naturally limits size  
**Unified**: Keep files small, focused, single-responsibility

**Enforcement**:
- 200-400 lines: ideal range
- 500 lines: warning threshold
- 600 lines: hard limit → refactor required

### 6. Memory Management

**Spec-Kit**: Feature-based specs prevent sprawl  
**Vibe-Coding**: Task-indexed archives keep memory slim  
**Unified**: Prevent context bloat through structured memory

## Principles Unique to Each

### Vibe-Coding Extensions

#### Resume Protocol
- Single active operation tracking
- Interruption recovery built-in
- Session state preserved across restarts

#### Progress Journal
- Past-tense session history
- Decision documentation
- Evidence of completion

#### Minimal Structure
- Can operate without constitution
- Lightweight for solo developers
- Fast iteration without governance overhead

### Spec-Kit Extensions

#### Constitution-Driven Governance
- Explicit, version-controlled principles
- Team-shared development philosophy
- Amendments require documentation and review

#### Research Phase
- Parallel research agents for tech validation
- Library compatibility checking
- Performance benchmarking before implementation

#### Structured Clarification
- Sequential, coverage-based questioning
- Clarifications section in specs
- Reduces rework downstream

## Unified Quality Standards

### Code Quality
- ✅ Clarity over cleverness
- ✅ Minimal dependencies
- ✅ Short, single-responsibility functions
- ✅ Public APIs documented
- ✅ Defensive input validation
- ✅ Safe logging (no sensitive data)

### Testing Standards
- ✅ Fast, hermetic by default
- ✅ Integration/e2e are opt-in
- ✅ Tests define acceptance criteria
- ✅ Tests written before implementation

### Security Standards
- ✅ Never commit secrets
- ✅ Use environment variables
- ✅ Provide .env.example files
- ✅ Input validation on all boundaries

### Process Standards
- ✅ Tasks executed in plan order
- ✅ No reordering without updating plan
- ✅ Small, scoped commits
- ✅ Checkpoint before milestone merges

## Decision Framework

When making technical decisions, consider in order:

1. **Constitution/Principles** — Does this align with core principles?
2. **Specifications** — Does this meet the stated requirements?
3. **Test-First** — Can this be tested? Write tests first.
4. **Simplicity** — Is this the simplest approach that works?
5. **Size** — Will this keep files under 600 lines?
6. **Observability** — Can this be debugged via text I/O?
7. **Security** — Does this introduce vulnerabilities?

## Acceptance Protocol

A task is "done" when ALL apply:

### Functional
- ✅ Acceptance criteria from spec/plan met
- ✅ Manually verified functionality works
- ✅ Edge cases handled appropriately

### Technical
- ✅ Tests written (if test-first)
- ✅ All tests pass (if tests exist)
- ✅ Linter/formatter pass (if configured)
- ✅ No security issues introduced

### Documentation
- ✅ Progress/plan updated with completion
- ✅ Decisions/caveats documented
- ✅ Evidence recorded ("✅ Done: <id> — evidence")
- ✅ Next steps clear

### Quality
- ✅ Files under 600 lines (or split planned)
- ✅ Code follows style conventions
- ✅ No secrets committed
- ✅ Public APIs documented (if applicable)

## File Organization

### Standalone Mode
```
_ai/
  memory/
    prd.md              # Lightweight requirements
    plan.md             # Phases → Tasks → Steps
    progress.md         # Session journal
    resume.md           # Active operation
    techenv.md          # Toolchain
    archive/            # Task-indexed archives
```

### Spec-Kit Mode
```
.specify/
  memory/
    constitution.md     # Project principles
  specs/
    NNN-feature/
      spec.md           # Requirements + user stories
      plan.md           # Implementation plan
      tasks.md          # Task breakdown
      research.md       # Tech validation
      contracts/        # API specs
```

### Hybrid Mode
Both structures coexist with bidirectional sync.

## Evolution Strategy

### Start Small (Standalone)
1. Begin with `v.createprd` for lightweight spec
2. Use `v.*` commands for rapid iteration
3. Minimal structure, maximum velocity

### Grow Structured (Spec-Kit)
1. **Option A**: Run `v.speckit init` when team/complexity grows (existing projects)
2. **Option B**: Use `specify init` CLI for fresh Spec-Kit projects (requires installation)
3. Create constitution with `v.speckit constitution` or `/speckit.constitution`
4. Use `/speckit.*` commands for governance

**Installing Spec-Kit CLI** (optional):
```bash
uv tool install specify-cli --from git+https://github.com/github/spec-kit.git
```

### Maintain Flexibility (Hybrid)
1. Use Spec-Kit for major features
2. Use Vibe-Coding for quick fixes
3. Sync with `v.speckit sync`

## Summary

Both frameworks share the fundamental insight: **specifications should drive implementation**.

The difference is **level of structure**:
- **Vibe-Coding**: Minimal structure for speed
- **Spec-Kit**: Full governance for teams
- **Hybrid**: Best of both worlds

Choose based on project needs, not dogma. Both approaches enforce quality through:
- Clear specifications
- Test-first development
- Explicit principles
- Size discipline
- Memory management
- Evidence-based acceptance

The tools adapt to your choice—you don't adapt to the tools.

---

**Version**: 1.0 (Unified principles from CLAUDE.md v2.3 + Spec-Kit constitution patterns)  
**Last Updated**: 2025-11-04  
© VibeLogic.app — Confidential / Proprietary
