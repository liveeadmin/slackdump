# v.feature — Manage Multiple Features in Standalone Mode

**Purpose**: Track and manage multiple features in Standalone mode, similar to Spec-Kit's feature tracking but with lightweight flexibility.

---

## Usage

```bash
v.feature <subcommand> [args]
```

**Subcommands**:
- `new <name>` — Create new feature
- `list` — List all features with status
- `switch <id>` — Switch to different feature
- `pause [id]` — Pause current or specified feature
- `resume <id>` — Resume paused feature
- `complete [id]` — Mark feature as complete
- `status` — Show current active feature
- `delete <id>` — Delete feature (with confirmation)

**Examples**:
```bash
v.feature new "authentication-system"
v.feature list
v.feature switch F-002
v.feature pause
v.feature resume F-001
v.feature status
```

---

## Feature Directory Structure

```
_ai/features/
├── active.md              # Current active feature
├── F-001-auth-system/
│   ├── spec.md           # Requirements
│   ├── plan.md           # Implementation plan
│   ├── tasks.md          # Task breakdown (optional)
│   └── progress.md       # Feature-specific progress
├── F-002-notifications/
│   ├── spec.md
│   ├── plan.md
│   └── progress.md
└── completed/            # Archived completed features
    └── F-001-auth-system/ (moved here when complete)
```

---

## Subcommands

### 1. Create New Feature (`v.feature new`)

```bash
v.feature new "photo-album-organizer"

# Creates:
# _ai/features/F-003-photo-album-organizer/
# _ai/features/F-003-photo-album-organizer/spec.md (empty template)
# Updates active.md to F-003

# Then use regular commands:
v.specify "Build photo album organizer..."  # Updates F-003/spec.md
v.plan "Use Vite, vanilla JS..."            # Creates F-003/plan.md
v.tasks                                      # Creates F-003/tasks.md
```

**Output**:
```
Created: _ai/features/F-003-photo-album-organizer/
Active feature: F-003

Next steps:
1. v.specify "Feature description" → Define requirements
2. v.plan "Tech stack" → Create implementation plan
3. v.tasks → Generate task breakdown
4. v.implement → Execute
```

### 2. List Features (`v.feature list`)

```bash
v.feature list

# Features

| ID    | Name                  | Status      | Progress | Branch              |
|-------|-----------------------|-------------|----------|---------------------|
| F-001 | auth-system           | Complete    | 100%     | main                |
| F-002 | notifications         | Paused      | 40%      | feature/notif       |
| F-003 | photo-album-organizer | In Progress | 25%      | feature/albums      |
| F-004 | search-functionality  | Draft       | 0%       | -                   |

Active: F-003 (photo-album-organizer)

Commands:
  v.feature switch F-002    # Resume notifications
  v.feature status          # Show active feature details
```

### 3. Switch Features (`v.feature switch`)

```bash
v.feature switch F-002

# Switching from F-003 to F-002...
# 
# Pausing: F-003 (photo-album-organizer)
# - Saved progress to F-003/progress.md
# - Saved resume state
#
# Activating: F-002 (notifications)
# - Status: Paused (40% complete)
# - Branch: feature/notif
# - Last task: T-005 "Implement WebSocket handler"
#
# Updated: _ai/features/active.md
#
# Next: v.resume (to continue last task)
#       or v.next (to see next task)
```

### 4. Pause Feature (`v.feature pause`)

```bash
v.feature pause

# Pausing active feature: F-003 (photo-album-organizer)
#
# Saved:
# - Current progress → F-003/progress.md
# - Resume state → F-003/resume.md
# - Task state → F-003/tasks.md
#
# Status changed: In Progress → Paused
#
# No active feature. Use 'v.feature switch' to work on another feature.
```

### 5. Resume Feature (`v.feature resume`)

```bash
v.feature resume F-002

# Same as v.feature switch F-002
# Sets F-002 as active
# Loads resume state
```

### 6. Complete Feature (`v.feature complete`)

```bash
v.feature complete F-003

# Marking F-003 as complete...
#
# Validation:
# ✅ All tasks completed
# ✅ Tests passing
# ✅ No resume.md (no work in progress)
# ✅ Branch merged to main
#
# Archiving:
# - Moving F-003/ → _ai/features/completed/F-003/
# - Updating feature index
# - Clearing active.md
#
# Feature F-003 (photo-album-organizer) is complete!
#
# Create new feature? (y/n): _
```

### 7. Feature Status (`v.feature status`)

```bash
v.feature status

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Active Feature: F-003
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Name: photo-album-organizer
Status: In Progress
Branch: feature/albums
Started: 2025-11-04
Progress: 25% (3/12 tasks)

Current Task: T-004 "Implement drag-drop reordering"
Files: src/components/album-list.js

Location: _ai/features/F-003-photo-album-organizer/

Files:
  ✅ spec.md (requirements defined)
  ✅ plan.md (implementation planned)
  ✅ tasks.md (12 tasks)
  🔄 progress.md (3 tasks complete)

Next: v.next → Show next task
      v.do → Execute current task
```

---

## Integration with Regular Commands

Commands automatically detect and use active feature:

### v.specify
```bash
v.specify "Build feature..."

# If active feature exists:
#   Updates _ai/features/F-003/spec.md
# Else:
#   Creates _ai/memory/prd.md (global)
```

### v.plan
```bash
v.plan "Tech stack..."

# If active feature exists:
#   Creates _ai/features/F-003/plan.md
# Else:
#   Updates _ai/memory/plan.md (global)
```

### v.tasks / v.implement / v.next / v.do
```bash
# All commands check active.md first
# Use feature files if active feature exists
# Fall back to global memory files otherwise
```

---

## Quick Tasks (Outside Feature Tracking)

For small tasks that don't need feature tracking:

```bash
# Option 1: Use --quick flag
v.specify "Fix validation bug" --quick
# Creates task in _ai/memory/plan.md, not a feature

# Option 2: No active feature
v.feature pause  # Clear active feature
v.specify "Quick fix for CSS issue"
# Uses _ai/memory/prd.md

# Option 3: Use global memory directly
# Edit _ai/memory/plan.md manually
# Run v.next → v.do
```

---

## Feature States

| State | Meaning | Can Work On? |
|-------|---------|--------------|
| **Draft** | Spec being written | Yes |
| **Planned** | Spec done, plan being created | Yes |
| **Ready** | Plan complete, ready to implement | Yes |
| **In Progress** | Implementation started | Yes |
| **Testing** | Implementation done, testing | Yes |
| **Paused** | Temporarily on hold | No (must resume) |
| **Complete** | Done and merged | No (archived) |

---

## Workflow Examples

### Multi-Feature Development

```bash
# Start first feature
v.feature new "authentication"
v.specify "Build JWT-based authentication"
v.plan "Use bcrypt, PostgreSQL users table"
v.tasks
v.implement  # Start implementation

# Need to switch to urgent feature
v.feature pause
v.feature new "hotfix-login-bug"
v.specify "Fix login redirect issue"
v.plan "Update redirect logic in auth.js"
v.do  # Quick fix

# Complete hotfix
v.feature complete F-002

# Resume original feature
v.feature switch F-001
v.resume  # Continue where left off
```

### Quick Task Without Feature Tracking

```bash
# Make sure no active feature
v.feature status  # Shows: No active feature

# Do quick task
v.specify "Update README with installation steps" --quick
# This updates _ai/memory/prd.md, not a feature

v.plan "Add npm install section with examples"
# Updates _ai/memory/plan.md

v.do  # Execute task

# Or even simpler: just add to plan.md manually
echo "## Task: Update README\n- Add installation steps" >> _ai/memory/plan.md
v.next
v.do
```

---

## Comparison: Features vs Global Memory

| Aspect | Features (`_ai/features/F-NNN/`) | Global Memory (`_ai/memory/`) |
|--------|----------------------------------|-------------------------------|
| **Use Case** | Multi-feature projects | Single-feature or quick tasks |
| **Structure** | One directory per feature | Single set of files |
| **Switching** | Can pause/resume features | One active task at a time |
| **Isolation** | Feature-specific progress | Shared progress |
| **Best For** | Complex projects, teams | Solo dev, simple projects |

---

## Migration to Spec-Kit

Features can be migrated to Spec-Kit:

```bash
v.speckit init

# Migrates:
# _ai/features/F-001/ → .specify/specs/001-auth-system/
# _ai/features/F-002/ → .specify/specs/002-notifications/
# _ai/features/active.md → .specify/active.md

# Feature IDs remain (F-001 → 001)
```

---

## Result

- Track multiple features in Standalone mode
- Lightweight alternative to Spec-Kit
- Switch between features easily
- Support quick tasks outside feature tracking
- Compatible with upgrade to Spec-Kit

## Notes

- Features are optional; use `_ai/memory/` for simple projects
- Active feature tracked in `active.md`
- Commands auto-detect and use active feature
- Can work on quick tasks without feature tracking
- Clean separation between features and global memory
