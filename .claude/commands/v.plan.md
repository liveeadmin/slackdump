# v.plan — Create Technical Implementation Plan

**Purpose**: Generate implementation plan from specification. Define **how** to build it with tech stack and architecture. Partially replaces `v.initproject`.

---

## Usage

```bash
v.plan [tech stack description]
```

**Examples**:
```bash
v.plan Use Vite with minimal libraries, vanilla HTML/CSS/JS, SQLite for local storage

v.plan Build with React + TypeScript, Node.js backend, PostgreSQL database, Docker deployment

v.plan Python FastAPI backend, React frontend, Redis for caching, deploy to AWS
```

---

## Behavior by Mode

### Spec-Kit Mode (if `.specify/specs/NNN-*/` exists)
Creates `.specify/specs/NNN-feature/plan.md` with:
- Technology choices with rationale
- Architecture overview
- Implementation phases
- File structure
- API contracts (if applicable)
- Data models

### Standalone Mode
Creates/updates `_ai/memory/plan.md` with:
- Phases → Tasks → Steps structure
- Tech stack summary
- File organization
- Acceptance criteria per step

---

## Implementation Plan Structure

### Full Plan (Spec-Kit Mode)

```markdown
# Implementation Plan: [Feature Name]

## Technology Stack

### Frontend
- **Framework**: [Choice] — [Rationale]
- **Styling**: [Choice] — [Rationale]
- **State**: [Choice] — [Rationale]

### Backend
- **Framework**: [Choice] — [Rationale]
- **Database**: [Choice] — [Rationale]
- **Auth**: [Choice] — [Rationale]

### Infrastructure
- **Hosting**: [Choice] — [Rationale]
- **CI/CD**: [Choice] — [Rationale]

## Architecture

### System Overview
[High-level architecture diagram/description]

### Components
1. [Component Name] — [Responsibility]
2. [Component Name] — [Responsibility]

### Data Flow
[How data moves through the system]

## Implementation Phases

### Phase 1: Foundation
**Goal**: [What we're building]
**Files**: [Where code goes]
**Tests**: [What to test]

### Phase 2: Core Features
...

## File Structure
```
project/
├─ src/
│  ├─ components/
│  └─ services/
├─ tests/
└─ docs/
```

## API Contracts
[API specifications, if applicable]

## Data Models
[Database schemas, data structures]
```

### Lightweight Plan (Standalone Mode)

```markdown
# Implementation Plan

## Tech Stack
- Frontend: [choices]
- Backend: [choices]
- Database: [choice]

## Phases

### PH-0: Setup
**Task T-001**: [Task name]
- Step S-001: [Specific action]
- Step S-002: [Specific action]
Acceptance: [How we verify completion]

**Task T-002**: [Task name]
...

### PH-1: Core Features
...
```

---

## Steps

1. **Read Specification**
   - Load spec.md or prd.md
   - Extract user stories
   - Identify technical requirements

2. **Parse Tech Stack Input**
   - Extract technology choices
   - Note constraints
   - Identify preferences (minimal deps, vanilla, etc.)

3. **Validate Tech Choices**
   - Check compatibility
   - Research versions (if needed)
   - Verify constraints can be met
   - Document rationale

4. **Design Architecture**
   - Map user stories to components
   - Define data models
   - Plan API contracts
   - Sketch file structure

5. **Break Into Phases**
   - Phase 0: Setup and scaffolding
   - Phase 1: Core features
   - Phase 2: Enhancement
   - Each phase has acceptance criteria

6. **Create Task Breakdown** (optional, or defer to `v.tasks`)
   - List tasks per phase
   - Define steps per task
   - Specify acceptance per step

7. **Document Rationale**
   - Why this tech stack?
   - Why this architecture?
   - What alternatives were considered?
   - What tradeoffs were made?

8. **Save Plan**
   - Write to appropriate location
   - Link from spec
   - Update progress tracker

---

## Tech Stack Guidance

### When to Use What

**Minimal/Vanilla** (User prefers simplicity):
- Vanilla JS over frameworks
- CSS over Tailwind/libraries
- SQLite over heavy databases
- No build tools if possible

**Modern/Productive** (User prefers tools):
- React/Vue for complex UIs
- TypeScript for safety
- Tailwind for rapid styling
- PostgreSQL for relational data

**Enterprise** (User needs scale):
- Microservices architecture
- Kubernetes deployment
- Redis caching
- Message queues

---

## Example Session

```bash
v.plan The application uses Vite with minimal libraries. Vanilla HTML, CSS, JavaScript. SQLite local database.

# Agent analyzes:
# - User wants lightweight approach
# - Local-first (SQLite = no backend)
# - Vite for dev tooling only
# - Minimal dependencies
#
# Generates:
#
# ## Technology Stack
# - **Build Tool**: Vite — Fast dev server, minimal config
# - **Frontend**: Vanilla JS — No framework overhead, direct DOM
# - **Styling**: Vanilla CSS — Full control, no library deps
# - **Database**: SQLite (sql.js) — Local storage, no backend
# - **Dependencies**: Only Vite and sql.js
#
# ## Architecture
# - Single-page application
# - Direct DOM manipulation
# - SQLite for persistence
# - No backend server
#
# ## File Structure
# src/
#   ├─ index.html
#   ├─ main.js
#   ├─ styles.css
#   ├─ db/
#   │  └─ database.js
#   └─ components/
#      ├─ albums.js
#      └─ photos.js
```

---

## Integration with Constitution

If constitution exists:
- Verify tech choices align with principles
- Apply library-first pattern (if required)
- Ensure test-first is planned
- Check observability requirements
- Validate against constraints

---

## Integration with Other Commands

**After planning**:
- Run `v.tasks` to generate detailed task breakdown
- Or manually proceed with `v.next` → `v.do`
- Use `v.review` to validate architecture

**Research Phase** (optional):
- For new/unfamiliar tech, spawn research tasks
- Validate version compatibility
- Check performance characteristics
- Document findings in `research.md`

---

## Result

- Detailed implementation plan
- Tech stack with rationale
- Architecture documented
- Phases and tasks defined
- Ready for task execution

## Relationship to v.initproject

- `v.initproject` — Bootstraps memory structure (progress.md, resume.md, etc.)
- `v.plan` — Creates technical implementation plan
- Often used together: `v.specify` → `v.plan` → `v.initproject` → `v.tasks`

## Notes

- Defer implementation details to code phase
- Focus on "what to build" not "exact syntax"
- Document rationale for future reference
- Plans evolve as understanding deepens
- Constitution (if exists) guides tech choices
