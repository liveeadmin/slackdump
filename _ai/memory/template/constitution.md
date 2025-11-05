# Project Constitution

This document contains project-specific development principles and standards. These rules govern how this specific project is built, separate from the framework rules in CLAUDE.md.

**Version**: 1.0  
**Created**: 2025-11-04  
**Status**: Draft (to be customized per project)

---

## Purpose

This constitution establishes immutable principles that guide all development decisions for this project. Changes to this document require explicit documentation and team agreement.

---

## Core Principles

### I. Specification-Driven Development

**Principle**: Specifications are the source of truth, not the implementation.

**Rules**:
- All features start with a specification (spec.md or prd.md)
- Implementation follows the specification, not personal preference
- Changes to requirements require updating the specification first
- Code that diverges from spec without updating spec is a bug

**Rationale**: Ensures alignment between intent and implementation. Makes codebase regenerable from specifications.

### II. Test-First Approach

**Principle**: Tests are written before implementation where feasible.

**Rules**:
- Unit tests precede implementation (Red → Green → Refactor)
- Acceptance criteria are expressed as tests
- Test coverage target: 80% minimum
- Tests must be fast (<100ms per unit test) and hermetic

**Rationale**: Tests define behavior explicitly. TDD catches issues early and improves design.

### III. File Size Discipline

**Principle**: Keep files small, focused, and maintainable.

**Rules**:
- Target: 200-400 lines per file
- Warning: 500 lines
- Hard limit: 600 lines → split required
- Single Responsibility Principle enforced

**Rationale**: Small files are easier to understand, test, and maintain. Forces good architecture.

### IV. Clarity Over Cleverness

**Principle**: Code should be obvious, not clever.

**Rules**:
- Clear naming (no abbreviations unless standard)
- Explicit over implicit
- Comment complex logic only
- Prefer readability over minor optimizations

**Rationale**: Code is read 10x more than written. Clarity reduces bugs and onboarding time.

### V. Minimal Dependencies

**Principle**: Minimize external dependencies.

**Rules**:
- Evaluate alternatives before adding dependency
- Prefer standard library over third-party
- Document rationale for each dependency
- Pin versions explicitly

**Rationale**: Dependencies increase complexity, security risk, and maintenance burden.

---

## Quality Standards

### Code Quality

- **Formatting**: Consistent style (use formatter)
- **Linting**: No linter warnings
- **Complexity**: Cyclomatic complexity < 10
- **Duplication**: No copy-paste code
- **Naming**: Descriptive and consistent

### Testing Standards

- **Coverage**: 80% minimum
- **Speed**: Unit tests < 100ms
- **Isolation**: No external dependencies in unit tests
- **Clarity**: Test names describe behavior
- **Assertions**: One logical assertion per test

### Documentation Standards

- **Public APIs**: Full documentation
- **README**: Up to date with examples
- **Comments**: Explain why, not what
- **Architecture**: Documented in plan.md
- **Decisions**: Recorded in progress.md

### Security Standards

- **Secrets**: Never in code (use env vars)
- **Input**: Always validated
- **Output**: Always encoded/sanitized
- **Auth**: Explicit and tested
- **Logging**: No sensitive data

### Performance Standards

- **Response Time**: < 200ms for API calls
- **Query Time**: < 50ms for database queries
- **Memory**: No obvious leaks
- **Caching**: Used appropriately
- **Optimization**: Profile before optimizing

---

## Architecture Patterns

### Module Organization

```
project/
├─ src/
│  ├─ models/      # Data structures
│  ├─ services/    # Business logic
│  ├─ api/         # External interfaces
│  └─ utils/       # Shared utilities
├─ tests/          # Test files mirror src/
└─ docs/           # Documentation
```

### Naming Conventions

- **Files**: kebab-case (user-service.js)
- **Classes**: PascalCase (UserService)
- **Functions**: camelCase (getUserById)
- **Constants**: UPPER_SNAKE_CASE (MAX_RETRIES)
- **Private**: prefix with _ (_internalMethod)

### Error Handling

- Use try-catch for all async operations
- Log errors with context
- Return meaningful error messages
- Never swallow errors silently
- Use custom error types for domain errors

### Logging Strategy

- **Levels**: debug, info, warn, error
- **Format**: Structured (JSON in production)
- **Content**: Context + message + timestamp
- **Sensitive**: Never log passwords, tokens, PII
- **Performance**: Use async logging

---

## Development Workflow

### Task Execution

1. Read task from plan.md or tasks.md
2. Write tests first (if test-first task)
3. Implement minimal solution
4. Verify acceptance criteria
5. Run linter and tests
6. Update progress.md with evidence
7. Commit with clear message

### Code Review Standards

- [ ] Tests pass
- [ ] Linter clean
- [ ] File sizes OK
- [ ] No security issues
- [ ] Documentation updated
- [ ] Acceptance criteria met
- [ ] Constitution followed

### Commit Standards

- **Format**: `type(scope): message`
- **Types**: feat, fix, docs, test, refactor, chore
- **Message**: Present tense, imperative mood
- **Size**: Small, focused commits
- **Frequency**: Commit early and often

---

## Amendment Process

This constitution can be amended with:

1. **Proposal**: Document the change with rationale
2. **Review**: Team discusses impacts
3. **Approval**: Consensus required
4. **Update**: Version number incremented
5. **Documentation**: Change recorded in CHANGELOG.md

**Current Version**: 1.0 (Initial draft)

---

## Relationship to CLAUDE.md

- **CLAUDE.md**: Framework rules (how to use v.* commands, session management, etc.)
- **constitution.md**: Project rules (how to build this specific project)

**CLAUDE.md is framework-level**. This file is project-level.

---

## Notes

- This is a template. Customize for your project's needs.
- Remove sections that don't apply.
- Add project-specific sections as needed.
- Keep it focused on principles, not implementation details.
- Review and update as project evolves.
