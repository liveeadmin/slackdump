# Features Directory

This directory tracks individual features in Standalone mode, similar to `.specify/specs/NNN-*` in Spec-Kit mode.

## Structure

```
_ai/features/
├── README.md (this file)
├── active.md (current active feature)
└── F-001-feature-name/
    ├── spec.md (what we're building)
    ├── plan.md (how we're building it)
    ├── tasks.md (task breakdown, optional)
    └── progress.md (feature-specific progress)
```

## Usage

### Create New Feature
```bash
v.specify "Build feature description"
# Creates _ai/features/F-NNN-feature-name/spec.md
# Updates active.md to point to F-NNN
```

### Switch Features
```bash
v.feature switch F-002
# Changes active.md to point to F-002
# Allows working on multiple features
```

### List Features
```bash
v.feature list
# Shows all features and their status
```

## Comparison with Spec-Kit

| Spec-Kit | Standalone | Purpose |
|----------|-----------|---------|
| `.specify/specs/001-name/` | `_ai/features/F-001-name/` | Feature directory |
| `spec.md` | `spec.md` | Requirements |
| `plan.md` | `plan.md` | Implementation plan |
| `tasks.md` | `tasks.md` | Task breakdown |
| Git branch `001-name` | Any branch | Feature branch |

## Active Feature

The `active.md` file tracks which feature is currently being worked on:

```markdown
# Active Feature: F-003-photo-albums

**Status**: In Progress
**Branch**: feature/photo-albums
**Started**: 2025-11-04
**Progress**: 45% (5/11 tasks complete)

See: _ai/features/F-003-photo-albums/
```

## Multiple Features

You can have multiple features simultaneously:
- One active feature (in active.md)
- Other features in different states (planned, paused, completed)
- Switch between features with `v.feature switch`

## Feature States

- **Draft** — Spec being written
- **Planned** — Spec complete, plan being created
- **Ready** — Plan complete, ready for implementation
- **In Progress** — Implementation started
- **Testing** — Implementation complete, testing in progress
- **Complete** — Feature done and merged
- **Paused** — Temporarily on hold

## Workflow

### Standalone Multi-Feature Workflow
```bash
# Create first feature
v.specify "Build authentication system"
# Creates F-001-authentication/

# Work on it
v.plan "Use JWT, bcrypt, PostgreSQL"
v.tasks
v.implement

# Pause and start another feature
v.feature pause F-001
v.specify "Build notification system"
# Creates F-002-notifications/

# Work on second feature
v.plan "Use WebSockets, Redis pub/sub"
v.tasks
v.implement

# Resume first feature
v.feature switch F-001
v.next → v.do
```

### Quick Tasks (Outside Feature Tracking)

For small tasks that don't need feature tracking:
```bash
# Use global memory files
_ai/memory/plan.md (for quick tasks)
_ai/memory/progress.md

# Or mark as quick-task
v.specify "Fix validation bug" --quick
# Creates task in plan.md, not a feature
```

## Notes

- Features are optional. For simple projects, use `_ai/memory/prd.md` + `plan.md` directly.
- For complex projects with multiple features, use feature directories.
- Active feature is tracked in `active.md` for easy resumption.
- Compatible with Spec-Kit: Can upgrade from features to Spec-Kit seamlessly.
