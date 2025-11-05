# v.specify — Define Requirements and Specifications

**Purpose**: Create or refine feature specifications. Focus on **what** you're building and **why**, not the tech stack. Replaces `v.createprd`.

---

## Usage

```bash
v.specify [feature description]
```

**Examples**:
```bash
v.specify Build an application that helps organize photos in albums grouped by date with drag-and-drop

v.specify Add OAuth2 authentication for GitHub and Google login
```

---

## Behavior by Mode

### Spec-Kit Mode (if `.specify/` exists)
1. Creates new feature branch (e.g., `003-photo-organizer`)
2. Creates `.specify/specs/003-photo-organizer/spec.md`
3. Structures with:
   - Feature overview
   - User stories
   - Acceptance criteria
   - Non-functional requirements
   - Out of scope

### Standalone Mode
1. Creates/updates `_ai/memory/prd.md`
2. Lightweight format:
   - Problem statement
   - MVP scope
   - V2/V3 features
   - Constraints
   - Simple acceptance tests

---

## Specification Structure

### Full Spec (Spec-Kit Mode)

```markdown
# [Feature Name]

## Overview
[What are we building and why]

## User Stories

### US-001: [User Story Title]
**As a** [user type]
**I want** [capability]
**So that** [benefit]

**Acceptance Criteria:**
- [ ] [Specific testable criteria]
- [ ] [Another criteria]

### US-002: [Next Story]
...

## Non-Functional Requirements
- Performance: [requirements]
- Security: [requirements]
- Accessibility: [requirements]

## Out of Scope
- [What we're NOT doing]

## Open Questions
- [ ] [Question to clarify]
```

### Lightweight Spec (Standalone Mode)

```markdown
# [Feature Name] PRD

## Mission
[One sentence: what and why]

## MVP Scope
[What must be in v1]

## Acceptance
[How we know it's done]

## V2/V3
[Future enhancements]

## Constraints
[Security, performance, compatibility]
```

---

## Steps

1. **Detect Mode**
   - Check for `.specify/` directory
   - Determine if using feature branches

2. **Create Feature Structure** (Spec-Kit mode only)
   - Find next feature number (e.g., 003)
   - Create git branch
   - Create spec directory
   - Copy spec template

3. **Generate Specification**
   - Parse user description
   - Extract user stories
   - Identify acceptance criteria
   - List non-functional requirements
   - Note open questions

4. **Interactive Clarification**
   - Present draft spec
   - Ask clarifying questions
   - Identify edge cases
   - Refine acceptance criteria
   - Document assumptions

5. **Save Specification**
   - Write to appropriate location
   - Format according to mode
   - Update index/references

6. **Suggest Next Step**
   - Standalone: "Run `v.plan` to create implementation plan"
   - Spec-Kit: "Run `/speckit.clarify` or `/speckit.plan`"

---

## Focus on WHAT and WHY

**Good Specifications:**
- ✅ "Users can drag photo albums to reorder them by date"
- ✅ "Albums show tile preview of first 4 photos"
- ✅ "System prevents nested albums for simplicity"

**Not Specifications (These are tech choices):**
- ❌ "Use React with Redux for state management"
- ❌ "Store data in PostgreSQL database"
- ❌ "Deploy with Docker containers"

**Tech stack comes later in `v.plan`!**

---

## Example Session

```bash
v.specify Build a task management app with Kanban boards

# Agent generates:
# 
# Specification Draft:
# 
# ## Overview
# A team productivity platform with Kanban-style task boards
#
# ## User Stories
# US-001: Create Projects
# As a project manager
# I want to create multiple projects
# So that I can organize work by initiative
# 
# US-002: Manage Tasks
# As a team member
# I want to move tasks between columns
# So that I can track progress visually
# 
# ... etc ...
#
# Questions:
# 1. How many team members per project?
# 2. Should tasks have assignees?
# 3. Do you need task comments/attachments?

# User answers questions, agent refines spec
```

---

## Integration with Other Commands

After specification exists:
- **Next**: Run `v.plan` to define tech stack
- **Or**: Run `/speckit.clarify` for structured questions (Spec-Kit mode)
- **Then**: Run `v.tasks` to break into actionable work
- **Finally**: Run `v.implement` to build it

---

## Result

- Clear specification document
- User stories with acceptance criteria
- Testable requirements
- Ready for technical planning
- Team has shared understanding

## Alias

- `v.specify` is the new primary name
- `v.createprd` still works (aliased for backward compatibility)

## Notes

- Focus on user value, not implementation
- Acceptance criteria must be testable
- Out-of-scope is as important as in-scope
- Open questions prevent assumptions
- Specifications evolve through clarification
