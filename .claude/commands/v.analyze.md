# v.analyze — Cross-Artifact Consistency Analysis

**Purpose**: Validate consistency and coverage across specification, plan, and tasks. Identifies gaps, conflicts, and misalignments before implementation.

---

## Usage

```bash
v.analyze [options]
```

**Options**:
- No arguments: Analyze all artifacts
- `--spec`: Analyze specification only
- `--plan`: Analyze plan only
- `--tasks`: Analyze tasks only
- `--report`: Generate detailed report

**Examples**:
```bash
v.analyze                    # Full analysis
v.analyze --spec             # Spec only
v.analyze --report > analysis.md   # Save report
```

---

## Behavior by Mode

### Spec-Kit Mode (if `.specify/specs/NNN-*/` exists)
Analyzes:
- spec.md (requirements)
- plan.md (implementation plan)
- tasks.md (task breakdown)
- constitution.md (principles)
- contracts/ (API specs if present)

### Standalone Mode
Analyzes:
- prd.md (requirements)
- plan.md (plan)
- CLAUDE.md (principles)

---

## Analysis Categories

### 1. Coverage Analysis

**Checks**:
- ✅ Every user story has implementation plan
- ✅ Every requirement has tasks
- ✅ Every acceptance criterion is testable
- ✅ Every task maps to requirement

**Flags**:
- ❌ User stories without tasks
- ❌ Tasks without user stories
- ❌ Requirements without acceptance
- ❌ Orphaned technical decisions

**Example output**:
```
Coverage Analysis
════════════════

✅ User Stories: 5/5 have implementation
❌ User Story US-003 missing acceptance criteria
✅ All tasks map to requirements
❌ Task 2.5 references non-existent component
⚠️  Plan includes "Redis" but no task sets it up

Coverage Score: 85% (4 issues found)
```

### 2. Consistency Analysis

**Checks**:
- Tech stack consistent across all files
- Data models align between spec and plan
- API contracts match implementation
- File paths are valid and consistent
- Dependencies are two-way consistent

**Flags**:
- ❌ Spec says "PostgreSQL", plan says "MySQL"
- ❌ Spec mentions "User.email", plan has "User.emailAddress"
- ❌ Task references "auth-service.js", plan says "auth.js"
- ❌ Spec requires "real-time", plan has no WebSocket/SSE

**Example output**:
```
Consistency Analysis
═══════════════════

❌ Database Mismatch:
   spec.md line 45: "SQLite for local storage"
   plan.md line 12: "PostgreSQL database"
   → Resolution: Update plan to use SQLite

✅ File paths consistent
✅ Component names match

⚠️  API Version Mismatch:
   spec.md: "REST API"
   plan.md: "GraphQL API"
   → Clarification needed

Consistency Score: 78% (2 conflicts, 1 warning)
```

### 3. Completeness Analysis

**Checks**:
- All WHAT requirements have HOW plans
- All technical decisions have rationale
- All dependencies are explicit
- All edge cases considered
- All acceptance criteria defined

**Flags**:
- ❌ No error handling plan
- ❌ Performance targets undefined
- ❌ Security requirements missing
- ❌ Test strategy not documented
- ⚠️  Deployment not mentioned

**Example output**:
```
Completeness Analysis
════════════════════

Requirements Coverage:
✅ Functional: 12/12 covered
❌ Non-functional: 2/8 covered
   Missing: Performance targets, Security model

Implementation Detail:
✅ Data models defined
✅ API contracts specified
❌ Error handling strategy missing
❌ Testing approach undefined

Edge Cases:
⚠️  No plan for empty state
⚠️  No plan for network failure
⚠️  No plan for concurrent edits

Completeness Score: 65% (5 gaps found)
```

### 4. Constitution Compliance

**Checks** (if constitution exists):
- Library-first pattern followed
- Test-first approach planned
- File size limits considered
- Observability requirements met
- Security principles applied

**Flags**:
- ❌ No library extraction planned
- ❌ Tests not mentioned
- ⚠️  Some files may exceed 600 lines
- ❌ No logging/monitoring in plan

**Example output**:
```
Constitution Compliance
══════════════════════

✅ Library-First: Services are separate modules
❌ Test-First: No test tasks before implementation
✅ CLI Interface: Each component has CLI
⚠️  File Size: Album component may exceed limit (est. 650 lines)
❌ Observability: No logging strategy

Compliance Score: 60% (3 violations)
```

### 5. Dependency Analysis

**Checks**:
- Task dependencies are acyclic
- All dependencies are defined
- Parallel tasks are truly independent
- Critical path identified
- Blocking dependencies flagged

**Flags**:
- ❌ Circular dependency: Task 2.3 ← 2.5 ← 2.3
- ❌ Task 3.1 depends on undefined Task 2.7
- ⚠️  Task 4.2 marked [P] but depends on 4.1

**Example output**:
```
Dependency Analysis
══════════════════

Task Graph:
✅ No circular dependencies
✅ All dependencies defined
⚠️  3 tasks marked parallel but have dependencies

Critical Path: T1.1 → T1.2 → T2.1 → T2.3 → T3.1
Estimated: 18 hours

Blocking Tasks:
- Task 1.1: Blocks 5 other tasks
- Task 2.1: Blocks 3 other tasks

Parallel Opportunities:
- Tasks 1.3, 1.4, 1.5 can run together (6h → 2h)
- Tasks 2.2, 2.4 can run together (4h → 2h)

Dependency Score: 90% (1 warning)
```

### 6. Quality Metrics

**Checks**:
- Requirements clarity score
- Technical detail level
- Test coverage plan
- Documentation completeness

**Example output**:
```
Quality Metrics
══════════════

Requirements Quality:
- Clarity: 85% (few ambiguities)
- Testability: 90% (clear acceptance)
- Completeness: 75% (some gaps)

Technical Quality:
- Detail Level: 80% (mostly complete)
- Rationale: 70% (some missing)
- Testability: 60% (needs improvement)

Overall Quality Score: 77%
```

---

## Full Analysis Report

```bash
v.analyze --report

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📊 Analysis Report: Photo Organizer
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Project: 003-photo-organizer
Analyzed: spec.md, plan.md, tasks.md
Date: 2025-11-04

Overall Score: 78% (Good)

═══════════════════════════════════════
1. Coverage Analysis
═══════════════════════════════════════

User Stories: 5
Tasks: 23
Coverage: 85%

✅ All user stories have tasks
❌ 2 user stories missing acceptance criteria
⚠️  3 technical requirements not in tasks

═══════════════════════════════════════
2. Consistency Analysis
═══════════════════════════════════════

Consistency: 78%

❌ CRITICAL: Database mismatch
   spec.md: SQLite
   plan.md: PostgreSQL
   → Action: Align on SQLite (per spec)

⚠️  WARNING: API approach unclear
   spec.md: mentions "REST"
   plan.md: no API section
   → Action: Clarify if API needed

✅ All other references consistent

═══════════════════════════════════════
3. Completeness Analysis
═══════════════════════════════════════

Completeness: 65%

Missing:
❌ Error handling strategy
❌ Performance targets (response time, data volume)
❌ Security model
❌ Testing approach
⚠️  Deployment plan

═══════════════════════════════════════
4. Constitution Compliance
═══════════════════════════════════════

Compliance: 60%

❌ Test-first not planned
✅ Library structure looks good
⚠️  File size may be an issue
❌ No observability plan

═══════════════════════════════════════
5. Dependency Analysis
═══════════════════════════════════════

Dependencies: 90%

✅ No circular dependencies
✅ Critical path identified (18 hours)
⚠️  Some parallel tasks have hidden dependencies

═══════════════════════════════════════
6. Quality Metrics
═══════════════════════════════════════

Quality: 77%

Requirements: Clear and testable
Technical: Good detail, some rationale missing
Tests: Needs more explicit test planning

═══════════════════════════════════════
Recommendations
═══════════════════════════════════════

Priority 1 (Must Fix):
1. Resolve database inconsistency
2. Add error handling strategy
3. Define test-first approach

Priority 2 (Should Fix):
4. Document performance targets
5. Add security model
6. Plan for observability

Priority 3 (Nice to Have):
7. Deployment considerations
8. More detailed rationale

═══════════════════════════════════════
Next Steps
═══════════════════════════════════════

✓ Fix Priority 1 items before implementation
→ Re-run v.analyze to verify fixes
→ Proceed with v.implement when score > 85%

═══════════════════════════════════════
```

---

## Integration with Other Commands

**Before v.analyze**:
- Must have `v.specify` (specification)
- Must have `v.plan` (implementation plan)
- Optionally `v.tasks` (task breakdown)
- Optionally `v.clarify` (clarifications)

**After v.analyze**:
- Fix identified issues
- Re-run `v.analyze` to verify
- Proceed with `v.implement` when score > 85%

**Continuous use**:
- Run after major spec updates
- Run after plan changes
- Run before implementation
- Run before checkpoints

---

## Analysis Rules

### Critical Issues (Must Fix)
- Inconsistent data models
- Missing core functionality
- Circular dependencies
- Conflicting requirements
- Undefined critical components

### Warnings (Should Fix)
- Missing rationale
- Unclear dependencies
- Potential file size issues
- Missing edge cases
- Incomplete documentation

### Info (Nice to Fix)
- Minor inconsistencies
- Style issues
- Optimization opportunities
- Documentation enhancements

---

## Result

- Comprehensive consistency report
- Identified gaps and conflicts
- Prioritized fix list
- Quality score
- Readiness assessment
- Actionable recommendations

## Notes

- Run before `v.implement` to catch issues early
- Re-run after fixing issues to verify
- Score > 85% recommended before implementation
- Can catch problems that would cause rework
- Especially valuable for team projects
- Automated "second pair of eyes"
