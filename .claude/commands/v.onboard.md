# v.onboard — Reverse Engineer Existing Project

**Purpose**: Analyze an existing codebase to understand its architecture, goals, and context, then generate all necessary AI memory files (PRD, plan, tasks, constitution, techenv) to continue development using the vibelogic "v." workflow.

---

## Usage

```bash
v.onboard [target-directory]
```

**Examples**:
```bash
v.onboard .
# Analyze current directory project

v.onboard /path/to/existing-project
# Analyze project in specified path

v.onboard . --deep
# Deep analysis including dependency graphs and test coverage
```

---

## What It Does

### Phase 1: Discovery
1. **Repository Analysis**
   - Scan project structure (directories, files, patterns)
   - Identify tech stack (languages, frameworks, tools)
   - Read package manifests (package.json, requirements.txt, go.mod, etc.)
   - Check build configs (webpack, vite, Makefile, etc.)
   - Detect testing frameworks and coverage

2. **Documentation Mining**
   - Read existing README, CONTRIBUTING, docs/
   - Extract purpose, features, and usage patterns
   - Identify known issues, TODOs, and roadmap items
   - Parse inline code comments for context

3. **Code Analysis**
   - Identify main entry points and core modules
   - Map module dependencies and data flow
   - Detect architectural patterns (MVC, microservices, etc.)
   - Find key abstractions and interfaces
   - List external integrations and APIs

4. **Git History Review**
   - Recent commit patterns and velocity
   - Active contributors and their focus areas
   - Unfinished branches or work-in-progress
   - Common bug patterns and fixes

### Phase 2: Knowledge Extraction
1. **Goal Identification**
   - What problem does this project solve?
   - Who are the users/stakeholders?
   - What's the value proposition?

2. **Use Case Mapping**
   - Primary workflows and user journeys
   - Edge cases and error handling
   - Integration points and dependencies

3. **Technical Architecture**
   - System components and their responsibilities
   - Data models and schemas
   - API contracts and interfaces
   - Security and authentication patterns

4. **Development Environment**
   - Setup instructions and prerequisites
   - Build, test, and deployment commands
   - Environment variables and configuration
   - Development tools and workflows

### Phase 3: AI Memory Generation

Creates complete vibelogic memory structure:

1. **`_ai/memory/constitution.md`**
   ```markdown
   # Project Constitution
   
   ## Coding Standards
   [Extracted from existing code patterns]
   
   ## Architecture Principles
   [Identified patterns and conventions]
   
   ## Testing Requirements
   [Current test coverage and standards]
   
   ## Security Policies
   [Authentication, authorization, data handling]
   ```

2. **`_ai/memory/prd.md`**
   ```markdown
   # [Project Name] Product Requirements
   
   ## Mission
   [One-sentence purpose extracted from docs/code]
   
   ## Current Features
   [What's already implemented]
   
   ## Known Issues
   [Bugs, TODOs, technical debt]
   
   ## Roadmap Items
   [Planned features from docs/issues]
   ```

3. **`_ai/memory/plan.md`**
   ```markdown
   # Implementation Plan
   
   ## PH-01: Stabilization
   ### T-001: Fix critical bugs
   - [ ] S-001: [Bug from TODO/issue]
   - [ ] S-002: [Another bug]
   
   ### T-002: Improve test coverage
   - [ ] S-001: Add tests for [uncovered module]
   
   ## PH-02: Feature Development
   ### T-003: [Next feature from roadmap]
   ```

4. **`_ai/memory/techenv.md`**
   ```markdown
   # Technical Environment
   
   ## Languages & Frameworks
   [Detected: Node.js 18, React 18, TypeScript 5, etc.]
   
   ## Build System
   \`\`\`bash
   npm run build    # Production build
   npm run dev      # Development server
   \`\`\`
   
   ## Testing
   \`\`\`bash
   npm test         # Run test suite
   npm run coverage # Coverage report
   \`\`\`
   
   ## Dependencies
   [Key libraries and their purposes]
   ```

5. **`_ai/memory/progress.md`**
   ```markdown
   # Progress Log
   
   ## 2025-11-04: Project Onboarded
   
   **Discovered**:
   - Project: [Name]
   - Purpose: [Mission]
   - Tech stack: [Summary]
   - Current state: [XX% complete, Y open issues]
   
   **Next Steps**:
   - Review constitution and plan
   - Prioritize tasks with user
   - Begin v.next → v.do workflow
   ```

### Phase 4: Interactive Proposal

After analysis, presents findings to user:

```
📊 PROJECT ANALYSIS COMPLETE

Project: MyAwesomeApp
Purpose: Social photo sharing with AI tagging
Tech Stack: Node.js 18, React 18, PostgreSQL, AWS S3
Current State: ~70% feature complete, 12 open TODOs

🎯 DISCOVERED GOALS:
1. Enable users to upload and organize photos
2. Auto-tag photos using ML
3. Share albums with friends

🏗️ KEY MODULES:
- auth/ — JWT-based authentication
- upload/ — S3 integration for photo storage  
- ml/ — TensorFlow.js tagging engine
- api/ — Express REST API

📋 IDENTIFIED TODOS:
- Fix upload timeout for large files (critical)
- Add pagination to album view
- Implement share link expiration
- Improve ML model accuracy

🛠️ TECH ENVIRONMENT:
Setup: npm install
Build: npm run build
Test: npm run test (current coverage: 65%)
Deploy: npm run deploy:prod

📁 GENERATED FILES:
✅ _ai/memory/constitution.md
✅ _ai/memory/prd.md
✅ _ai/memory/plan.md
✅ _ai/memory/techenv.md
✅ _ai/memory/progress.md

🎬 READY TO CONTINUE!

Suggested next steps:
1. Review the generated plan.md
2. Choose what to work on: v.next
3. Start development: v.do

What would you like to focus on?
```

---

## Analysis Modes

### Standard Mode (default)
- Scans up to 500 files
- Reads documentation and key source files
- Basic git history (last 50 commits)
- Generates core memory files

### Deep Mode (`--deep`)
- Unlimited file scanning
- Full dependency graph analysis
- Complete git history analysis
- Generates additional artifacts:
  - `_ai/memory/docs/architecture.md`
  - `_ai/memory/docs/api-reference.md`
  - `_ai/memory/docs/dependency-map.md`

### Quick Mode (`--quick`)
- Scans only top-level and key directories
- Skips git history
- Minimal documentation reading
- Generates basic structure only

---

## Smart Detection

The command intelligently identifies:

### Languages & Frameworks
```
Python: requirements.txt, setup.py, Pipfile
Node.js: package.json, yarn.lock
Go: go.mod, go.sum
Rust: Cargo.toml
Java: pom.xml, build.gradle
.NET: *.csproj, *.sln
Ruby: Gemfile, .gemspec
PHP: composer.json
```

### Project Types
```
Web App: index.html, src/components/
API Server: routes/, controllers/, api/
CLI Tool: bin/, cmd/, main entry point
Library: lib/, dist/, published package
Mobile: android/, ios/, React Native
Desktop: electron/, tauri/
```

### Testing Frameworks
```
Jest, Mocha, pytest, Go test, RSpec, JUnit, etc.
```

### Documentation Patterns
```
README.md, CHANGELOG.md, docs/, wiki/
API specs: OpenAPI, GraphQL schemas
Architecture: ADRs, C4 diagrams
```

---

## File Discovery Priorities

When scanning codebase, prioritizes reading:

1. **Root documentation** (README, CONTRIBUTING, ARCHITECTURE)
2. **Configuration files** (package.json, .env.example, config/)
3. **Entry points** (main.js, index.ts, app.py, main.go)
4. **Core modules** (most imported/referenced files)
5. **Tests** (to understand intended behavior)
6. **Recent changes** (git log --since="3 months ago")

Skips:
- node_modules/, vendor/, .git/ (except history)
- Build artifacts (dist/, build/, target/)
- Large binary files
- Generated code

---

## Integration with Existing AI Tools

If the project was built with a different AI coding tool:

### Cursor/Copilot Detection
- Looks for `.cursorrules`, `.github/copilot-instructions.md`
- Extracts rules and converts to constitution.md
- Preserves coding preferences and conventions

### Aider Detection
- Looks for `.aider.conf.yml`, `.aider/`
- Extracts context and converts to memory structure

### Other AI Tools
- Generic markdown documentation in project root
- Inline AI instructions in comments
- Custom .ai/ or similar directories

**Conversion Strategy**: Extract reusable context while adapting to vibelogic structure.

---

## Error Handling

### If Project Structure is Unclear
- Prompts user for clarification
- Offers multiple interpretations
- Allows manual override of detected tech stack

### If No Documentation Exists
- Generates skeleton based on code analysis
- Marks sections as "[INFERRED]" for user review
- Suggests running v.clarify after onboarding

### If Project is Too Large
- Suggests using --quick mode first
- Offers to analyze specific subdirectories
- Provides sampling strategy for large monorepos

---

## Workflow Integration

After `v.onboard`, the standard workflow applies:

```bash
# 1. Onboard existing project
v.onboard .

# 2. Review generated files
cat _ai/memory/prd.md
cat _ai/memory/plan.md

# 3. Optional: refine with clarifications
v.clarify "What's the ML model accuracy requirement?"

# 4. Start working
v.next
v.do

# 5. Continue as normal
v.checkpoint
```

---

## Output Structure

```
_ai/
  memory/
    constitution.md      ← Project-specific rules
    prd.md               ← Requirements and current state
    plan.md              ← Structured PH/T/S implementation plan
    techenv.md           ← Tech stack and commands
    progress.md          ← Initial onboarding entry
    resume.md            ← Empty (ready for work)
    archive/             ← Empty (ready for future archival)
    docs/                ← Optional: detailed docs (--deep mode)
      architecture.md
      api-reference.md
      dependency-map.md
```

---

## Best Practices

1. **Run Early**: Use v.onboard when picking up an unfamiliar project
2. **Review Generated Files**: AI inference isn't perfect—verify the extracted goals
3. **Refine Before Working**: Update plan.md priorities based on your needs
4. **Commit Memory Files**: Track AI context alongside code
5. **Re-run When Stuck**: If project evolves significantly, re-onboard to refresh context

---

## Example: Onboarding a React App

```bash
$ v.onboard ~/projects/photo-gallery

🔍 Analyzing project structure...
✅ Detected: React 18.2.0 + TypeScript + Vite
✅ Found: 42 components, 8 pages, 12 API routes
✅ Testing: Jest + React Testing Library (68% coverage)
✅ Documentation: README.md + docs/ folder

📖 Reading documentation...
✅ README: Photo gallery with AI tagging
✅ TODO comments: 15 items found
✅ Git history: 230 commits over 4 months

🏗️ Analyzing architecture...
✅ State management: Redux Toolkit
✅ Routing: React Router v6
✅ API: Axios + REST
✅ Styling: Tailwind CSS

💾 Generating memory files...
✅ _ai/memory/constitution.md (React + TS standards)
✅ _ai/memory/prd.md (Feature list + roadmap)
✅ _ai/memory/plan.md (15 tasks from TODOs)
✅ _ai/memory/techenv.md (npm scripts + env vars)
✅ _ai/memory/progress.md (Onboarding summary)

🎉 Project onboarded successfully!

Next steps:
  v.next     # See next task
  v.do       # Start working
  
Want me to start with the first TODO?
```

---

## Compatibility

- **Works with**: Any programming language, any framework
- **Adapts to**: Monorepo, multi-module, microservices, libraries
- **Mode-aware**: Creates Spec-Kit structure if `.specify/` exists
- **Non-destructive**: Never modifies existing code or docs
- **Incremental**: Can re-run to update memory as project evolves

---

## See Also

- **v.constitution** — Manually create/edit project rules
- **v.specify** — Manually define requirements
- **v.plan** — Generate implementation plan
- **v.clarify** — Ask clarifying questions after onboarding
- **v.analyze** — Validate consistency of generated memory files
