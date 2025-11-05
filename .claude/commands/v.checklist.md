# v.checklist — Generate Quality Validation Checklists

**Purpose**: Create custom quality checklists that validate requirements completeness, clarity, and consistency. Like "unit tests for English" — ensure specifications meet quality standards before implementation.

---

## Usage

```bash
v.checklist [type] [options]
```

**Types**:
- `spec` — Specification quality checklist
- `plan` — Implementation plan checklist
- `tasks` — Task breakdown checklist
- `code` — Code quality checklist
- `custom` — Custom checklist based on criteria

**Options**:
- `--template NAME` — Use specific template
- `--save` — Save checklist for reuse
- `--check` — Auto-check items where possible

**Examples**:
```bash
v.checklist spec                    # Spec quality checklist
v.checklist plan --check            # Plan checklist with auto-checking
v.checklist custom --template api   # API design checklist
```

---

## Checklist Types

### 1. Specification Quality Checklist

Validates requirements are:
- Clear and unambiguous
- Complete and comprehensive
- Consistent and non-contradictory
- Testable and verifiable
- Prioritized and scoped

**Generated checklist**:
```markdown
# Specification Quality Checklist

## Clarity (No Ambiguity)
- [ ] All requirements use concrete, specific language
- [ ] No subjective terms ("good", "fast", "easy", "intuitive")
- [ ] Quantities are specified with numbers and units
- [ ] Roles and permissions are explicitly defined
- [ ] Data types and formats are specified
- [ ] Error conditions are clearly described

## Completeness
- [ ] All user stories have acceptance criteria
- [ ] All features have "done" definition
- [ ] Edge cases are identified
- [ ] Error handling requirements are stated
- [ ] Performance requirements are specified
- [ ] Security requirements are documented
- [ ] Data model is complete

## Consistency
- [ ] Terminology is used consistently
- [ ] No contradictory requirements
- [ ] Data models align across requirements
- [ ] User flows are coherent
- [ ] No duplicate/overlapping requirements

## Testability
- [ ] Every requirement has measurable acceptance
- [ ] Success criteria are observable
- [ ] Edge cases can be tested
- [ ] Performance targets are quantified
- [ ] Security requirements are verifiable

## Scope & Priority
- [ ] MVP vs future features are distinguished
- [ ] Dependencies are identified
- [ ] Priorities are assigned
- [ ] Out-of-scope items are documented
- [ ] Timeline is realistic

## Stakeholder Alignment
- [ ] Requirements approved by stakeholders
- [ ] Open questions are resolved
- [ ] Assumptions are documented
- [ ] Constraints are acknowledged
```

### 2. Implementation Plan Checklist

Validates technical plans are:
- Architecture is sound
- Technology choices are justified
- All requirements are addressed
- Implementation is feasible

**Generated checklist**:
```markdown
# Implementation Plan Quality Checklist

## Technology Stack
- [ ] All technology choices have rationale
- [ ] Versions are specified
- [ ] Compatibility is verified
- [ ] Licenses are acceptable
- [ ] Team has expertise or learning path
- [ ] Performance characteristics understood

## Architecture
- [ ] System components are identified
- [ ] Component responsibilities are clear
- [ ] Interfaces are defined
- [ ] Data flow is documented
- [ ] Integration points are specified
- [ ] Scalability is addressed

## Data Model
- [ ] All entities are defined
- [ ] Relationships are specified
- [ ] Constraints are documented
- [ ] Indexes are planned
- [ ] Migrations are considered
- [ ] Data validation is defined

## Implementation Detail
- [ ] File structure is planned
- [ ] Module organization is clear
- [ ] Naming conventions are defined
- [ ] Error handling strategy exists
- [ ] Logging/monitoring is planned
- [ ] Testing strategy is documented

## Non-Functional Requirements
- [ ] Performance targets are addressed
- [ ] Security model is defined
- [ ] Scalability is planned
- [ ] Reliability/availability is considered
- [ ] Observability is included

## Risk Assessment
- [ ] Technical risks are identified
- [ ] Mitigation strategies exist
- [ ] Dependencies are manageable
- [ ] Complexity is justified
- [ ] Alternatives were considered
```

### 3. Task Breakdown Checklist

Validates tasks are:
- Actionable and atomic
- Correctly ordered
- Properly scoped
- Clearly defined

**Generated checklist**:
```markdown
# Task Breakdown Quality Checklist

## Task Definition
- [ ] Each task is actionable (clear "do X")
- [ ] Each task has acceptance criteria
- [ ] Each task specifies affected files
- [ ] Each task has time estimate
- [ ] Each task is sized appropriately (hours not days)

## Dependencies
- [ ] All dependencies are explicit
- [ ] No circular dependencies exist
- [ ] Critical path is identified
- [ ] Parallel opportunities are marked
- [ ] Blocking tasks are flagged

## Coverage
- [ ] All user stories have tasks
- [ ] All components have tasks
- [ ] Setup/configuration tasks exist
- [ ] Testing tasks are included
- [ ] Documentation tasks exist
- [ ] Deployment tasks are present

## Test-First
- [ ] Test tasks precede implementation
- [ ] Test and implementation tasks are paired
- [ ] Integration tests are planned
- [ ] E2E tests are planned (if needed)

## Quality Gates
- [ ] Checkpoints are defined
- [ ] Validation steps are included
- [ ] Review points are marked
- [ ] Acceptance tests are planned
```

### 4. Code Quality Checklist

Validates code meets standards:
- Follows conventions
- Is well-tested
- Is maintainable
- Is secure

**Generated checklist**:
```markdown
# Code Quality Checklist

## Code Structure
- [ ] Files are under 600 lines
- [ ] Functions are under 50 lines
- [ ] Single responsibility principle
- [ ] No code duplication
- [ ] Clear naming conventions
- [ ] Consistent formatting

## Testing
- [ ] Unit tests exist
- [ ] Tests pass
- [ ] Coverage > 80%
- [ ] Edge cases tested
- [ ] Error paths tested
- [ ] Integration tests pass

## Documentation
- [ ] Public APIs documented
- [ ] Complex logic commented
- [ ] README is up to date
- [ ] Examples provided
- [ ] Breaking changes noted

## Security
- [ ] No secrets in code
- [ ] Input validation present
- [ ] Output encoding correct
- [ ] Authentication works
- [ ] Authorization correct
- [ ] Security review done

## Performance
- [ ] No obvious bottlenecks
- [ ] Queries are optimized
- [ ] Caching is used appropriately
- [ ] Resources are released
- [ ] Memory leaks checked

## Maintainability
- [ ] Dependencies are minimal
- [ ] Error handling is consistent
- [ ] Logging is appropriate
- [ ] Code is readable
- [ ] Configuration is externalized
```

### 5. Custom Checklists

Create domain-specific checklists:

**API Design Checklist**:
```markdown
# API Design Quality Checklist

## REST Principles
- [ ] Resources are nouns, not verbs
- [ ] HTTP methods are used correctly
- [ ] Status codes are appropriate
- [ ] Idempotency is considered
- [ ] Versioning strategy exists

## Request/Response
- [ ] Request validation is defined
- [ ] Response format is consistent
- [ ] Error responses are standardized
- [ ] Pagination is included (where needed)
- [ ] Rate limiting is considered

## Documentation
- [ ] All endpoints are documented
- [ ] Request examples provided
- [ ] Response examples provided
- [ ] Error scenarios documented
- [ ] Authentication described

## Security
- [ ] Authentication method defined
- [ ] Authorization per endpoint
- [ ] CORS is configured
- [ ] HTTPS is enforced
- [ ] API keys/tokens are secure
```

**Database Design Checklist**:
```markdown
# Database Design Quality Checklist

## Schema Design
- [ ] Normalization is appropriate
- [ ] Primary keys are defined
- [ ] Foreign keys are constrained
- [ ] Indexes are planned
- [ ] Data types are optimal

## Data Integrity
- [ ] NOT NULL constraints where appropriate
- [ ] Unique constraints defined
- [ ] Check constraints added
- [ ] Default values set
- [ ] Cascading rules defined

## Performance
- [ ] Indexes on foreign keys
- [ ] Indexes on query columns
- [ ] Avoid premature optimization
- [ ] Large columns separated
- [ ] Query patterns optimized

## Migrations
- [ ] Migration scripts exist
- [ ] Rollback scripts exist
- [ ] Data migration planned
- [ ] Version control included
- [ ] Testing strategy defined
```

---

## Auto-Checking

When `--check` flag is used, automatically validates checkable items:

```bash
v.checklist spec --check

# Specification Quality Checklist (Auto-Checked)

## Clarity
✅ All requirements use concrete language (automated scan passed)
❌ Subjective terms found: "user-friendly" (line 23), "fast" (line 45)
✅ Quantities specified: 5/5 requirements
⚠️  Role "admin" used but not defined
✅ Data types specified in data model
❌ Error condition "network failure" not described

Auto-check: 3/6 passed, 2 failed, 1 warning
Manual review required for: Completeness, Consistency, Testability
```

---

## Example Session

```bash
v.checklist spec

# Generated: .specify/specs/003-photo-organizer/spec-checklist.md

Specification Quality Checklist created!

Location: .specify/specs/003-photo-organizer/spec-checklist.md

Review and check off items to ensure quality.
Run 'v.checklist spec --check' to auto-validate where possible.

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Specification Quality Summary
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Clarity: 0/6 checked
Completeness: 0/7 checked
Consistency: 0/5 checked
Testability: 0/4 checked
Scope & Priority: 0/5 checked

Total: 0/27 items

Next: Review spec.md against this checklist
```

---

## Integration with Other Commands

**After specification**:
- Run `v.checklist spec` to validate spec quality
- Fix issues before proceeding to `v.plan`

**After planning**:
- Run `v.checklist plan` to validate plan
- Ensure technical soundness before `v.tasks`

**After task breakdown**:
- Run `v.checklist tasks` to validate tasks
- Verify task quality before `v.implement`

**During implementation**:
- Run `v.checklist code` for code review
- Use before `v.checkpoint`

**Custom scenarios**:
- Create API checklist before API development
- Create security checklist before security review
- Create UX checklist before UI implementation

---

## Checklist Templates

Available templates:
- `spec-quality` — Specification validation
- `plan-quality` — Implementation plan validation
- `task-quality` — Task breakdown validation
- `code-quality` — Code review checklist
- `api-design` — REST API design
- `database-design` — Database schema
- `security-review` — Security audit
- `ux-design` — User experience
- `performance` — Performance optimization
- `accessibility` — Accessibility compliance

---

## Custom Template Creation

```bash
v.checklist custom --template my-checklist

# Prompts for:
# - Checklist purpose
# - Categories
# - Items per category
# - Auto-check criteria

# Generates custom template
# Saves for reuse
```

---

## Result

- Quality validation checklist
- Auto-checked items (where possible)
- Manual review items
- Quality score/summary
- Saved for team reuse
- Prevents quality issues early

## Notes

- Checklists are "unit tests for specs"
- Catch quality issues before implementation
- Standardize quality across team
- Templates are reusable
- Auto-checking saves time
- Manual review still essential
- Score > 90% recommended before proceeding
