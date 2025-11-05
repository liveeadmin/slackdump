# v.speckit — Spec-Kit Integration Bridge

**Purpose**: Enable Spec-Kit compatibility mode and bridge between vibe-coding and spec-driven workflows.

---

## Usage Modes

### 1. Initialize Spec-Kit Mode
Convert existing `_ai/memory/` project to Spec-Kit structure:
```
v.speckit init
```
This will:
- Create `.specify/` directory structure
- Migrate `_ai/memory/prd.md` → `.specify/specs/001-initial/spec.md`
- Migrate `_ai/memory/plan.md` → `.specify/specs/001-initial/plan.md`
- Create constitution template if missing
- Set up scripts for feature-branch workflow

### 2. Check Compatibility
Verify which mode is active and show available commands:
```
v.speckit status
```
Shows:
- Current mode (Standalone vs Spec-Kit)
- Active feature/branch
- Available commands for current mode
- Path mappings

### 3. Create Constitution
Generate a constitution from current practices:
```
v.speckit constitution
```
Analyzes existing code/patterns and suggests:
- Core development principles
- Quality standards
- Testing requirements
- Architecture patterns

### 4. Sync Bi-directionally
Keep both structures in sync (when both exist):
```
v.speckit sync
```
- Updates `.specify/specs/*/` from `_ai/memory/`
- Or updates `_ai/memory/` from `.specify/specs/*/`
- Maintains compatibility between modes

---

## Implementation Steps

1. **Detect Current Mode**
   ```bash
   if [ -d ".specify" ]; then
     MODE="speckit"
   elif [ -d "_ai/memory" ]; then
     MODE="standalone"
   else
     MODE="new"
   fi
   ```

2. **For `init` Command**
   - Create `.specify/` structure
   - Copy speckit scripts from reference
   - Migrate existing files
   - Update CLAUDE.md with feature info

3. **For `status` Command**
   - Show current mode
   - List active specs/features
   - Show command mappings
   - Suggest next actions

4. **For `constitution` Command**
   - Analyze existing codebase patterns
   - Check for CLAUDE.md principles
   - Generate constitution template
   - Place in `.specify/memory/constitution.md`

5. **For `sync` Command**
   - Detect which is source of truth (newer timestamp)
   - Copy/merge content intelligently
   - Preserve unique sections in each
   - Log sync actions

---

## Path Mappings

### Standalone → Spec-Kit
- `_ai/memory/prd.md` → `.specify/specs/NNN-feature/spec.md`
- `_ai/memory/plan.md` → `.specify/specs/NNN-feature/plan.md`
- `_ai/memory/progress.md` → (stays as session journal)
- `_ai/memory/resume.md` → (stays as active operation)

### Constitution Creation
- Extract principles from `CLAUDE.md` Core Behavior Rules
- Convert to `.specify/memory/constitution.md` format
- Link file size caps, testing rules, etc.

### Feature Workflow
- Each new feature → new branch + `.specify/specs/NNN-name/`
- Commands auto-detect and use appropriate paths
- Resume/progress still track in `_ai/memory/` for session state

---

## Decision Logic

Commands should detect mode and adapt:

```python
def get_spec_path():
    if exists('.specify/'):
        # Find active feature from branch name
        branch = git_branch()
        match = re.match(r'(\d+)-', branch)
        if match:
            num = match.group(1)
            return f'.specify/specs/{num:03d}-*/spec.md'
    return '_ai/memory/prd.md'

def get_plan_path():
    if exists('.specify/'):
        return f'.specify/specs/{active_feature()}/plan.md'
    return '_ai/memory/plan.md'
```

---

## Result
- Seamless operation in either mode
- Ability to upgrade from standalone → Spec-Kit
- Ability to sync between both if needed
- Commands work regardless of mode

## Notes
- Spec-Kit mode recommended for teams and large projects
- Standalone mode great for personal projects and rapid iteration
- Constitution is optional but highly recommended
- Can run both modes simultaneously with sync
