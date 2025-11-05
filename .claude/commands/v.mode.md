# v.mode — Mode Management
**Purpose**: Manage framework operating mode (Standalone vs Spec-Kit)
**Version**: 1.0.0 | **Last Updated**: 2025-11-04

## Usage

```bash
v.mode status              # Show current mode
v.mode enable speckit      # Enable Spec-Kit mode
v.mode disable speckit     # Disable Spec-Kit mode
```

## Commands

### v.mode status

Shows current operating mode and configuration.

**Output**:
```
Current Mode: Standalone / Spec-Kit / Feature-Based Standalone
Detection: Based on .specify/.enabled existence

Spec-Kit Status:
  .speckit/ directory: ✅ Present / ❌ Not found
  .specify/ directory: ✅ Present / ❌ Not found
  .specify/.enabled: ✅ Present / ❌ Not found
  
Active Mode: [Standalone | Spec-Kit]

To switch modes:
  v.mode enable speckit   - Activate Spec-Kit mode
  v.mode disable speckit  - Return to Standalone mode
```

### v.mode enable speckit

Activates Spec-Kit mode by creating `.specify/.enabled` marker file.

**Prerequisites**:
- `.speckit/` directory must exist
- If not, suggest: `cp -r path/to/framework/.speckit .`

**Actions**:
1. Check if `.speckit/` exists
2. Create `.specify/` directory if missing
3. Create `.specify/.enabled` marker file
4. Suggest initializing with `v.constitution` if `.specify/memory/constitution.md` missing

**Output**:
```
✅ Spec-Kit mode enabled

Next steps:
1. Initialize constitution: v.constitution
2. Create specification: v.specify
3. Create plan: v.plan
4. Execute: v.implement

Files will be created in .specify/ instead of _ai/memory/
```

**Git-friendly**: 
- `.specify/.enabled` is a small marker file (safe for git)
- No directory renaming (avoids git tracking issues)

### v.mode disable speckit

Deactivates Spec-Kit mode by renaming marker file to `.specify/.disabled`.

**Actions**:
1. Check if `.specify/.enabled` exists
2. Rename `.specify/.enabled` → `.specify/.disabled`
3. Return to Standalone mode

**Output**:
```
✅ Spec-Kit mode disabled

Switched to Standalone mode.
Commands will now use _ai/memory/ instead of .specify/

Data preserved in .specify/ (disabled, not deleted)

To re-enable: v.mode enable speckit
```

**Data Safety**:
- `.specify/` directory and all content preserved
- Only marker file renamed
- Easy to re-enable without data loss

## Mode Detection Logic (Updated)

Framework detects mode in this order:

1. **IF `.specify/.enabled` exists** → **Spec-Kit mode**
   - Use `.specify/specs/`, `.specify/memory/constitution.md`
   - Commands adapt to Spec-Kit workflow

2. **ELSE IF `_ai/features/active.md` exists** → **Feature-Based Standalone**
   - Use `_ai/features/F-NNN/` for active feature
   - Global memory in `_ai/memory/`

3. **ELSE** → **Simple Standalone**
   - Use `_ai/memory/plan.md`, `_ai/memory/progress.md`
   - Single-feature workflow

## Workflow Examples

### Testing Spec-Kit Mode

```bash
# Initial setup (Standalone)
cp -r framework/_ai .
cp -r framework/.claude .
cp -r framework/.speckit .
cp framework/CLAUDE.md .

# Work in Standalone
v.specify → creates _ai/memory/prd.md
v.plan → creates _ai/memory/plan.md

# Try Spec-Kit
v.mode enable speckit
v.constitution
v.specify → creates .specify/specs/001-feature/spec.md
v.plan → creates .specify/specs/001-feature/plan.md

# Compare workflows, return to Standalone
v.mode disable speckit
v.next → reads from _ai/memory/plan.md

# Re-enable Spec-Kit (data preserved)
v.mode enable speckit
v.next → reads from .specify/specs/*/tasks.md
```

### Copying Framework to New Project

```bash
# Copy everything
cp -r framework/_ai .
cp -r framework/.claude .
cp -r framework/.speckit .  # Available but inactive
cp framework/{CLAUDE.md,AGENTS.md,GEMINI.md} .

# Check mode
v.mode status
# Output: Standalone (default)

# Work normally with v.* commands
v.specify → _ai/memory/prd.md
v.plan → _ai/memory/plan.md
v.implement

# Later, try Spec-Kit if curious
v.mode enable speckit
```

## Implementation Details

### .specify/.enabled marker file

Content example:
```
Spec-Kit mode enabled
Created: 2025-11-04
```

This small file:
- ✅ Git-friendly (trackable, no binary issues)
- ✅ No directory renaming (avoids git confusion)
- ✅ Clear intent (explicit enable/disable)
- ✅ Preserves all data in `.specify/`

### Git Integration

Add to `.gitignore` (optional):
```gitignore
# Spec-Kit mode markers (team decision)
.specify/.enabled
.specify/.disabled
```

Or commit `.enabled` to ensure team uses same mode.

## Troubleshooting

### "Spec-Kit mode not activating"

Check:
```bash
v.mode status
ls -la .specify/
```

Ensure `.specify/.enabled` exists (not `.disabled`)

### "Commands using wrong directory"

```bash
v.mode status  # Verify mode
# If wrong, toggle mode:
v.mode disable speckit  # or enable
```

### "Lost data after switching"

Data never deleted, only mode changes. Check:
- Standalone data: `_ai/memory/`
- Spec-Kit data: `.specify/specs/`

Both directories preserved independently.

## Notes

- `.speckit/` can be copied to all projects (dormant until activated)
- Mode switching is instant (just marker file)
- Data from both modes preserved (no destructive operations)
- Can switch modes anytime to compare workflows
- Agent automatically detects mode on each session start
