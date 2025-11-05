# v.clarify — Clarify Underspecified Requirements

**Purpose**: Systematically identify and resolve ambiguities, gaps, and underspecified areas in the specification before creating implementation plan. Reduces downstream rework.

---

## Usage

```bash
v.clarify
```

No arguments needed — reads from existing specification.

---

## Behavior by Mode

### Spec-Kit Mode (if `.specify/specs/NNN-*/spec.md` exists)
- Analyzes spec.md for ambiguities
- Creates "Clarifications" section in spec.md
- Uses structured questioning approach
- Records all Q&A in specification

### Standalone Mode
- Analyzes prd.md for gaps
- Adds clarifications inline or in separate section
- Less formal but thorough questioning
- Updates prd.md with answers

---

## Clarification Process

### 1. Analysis Phase

**What gets analyzed**:
- User stories without acceptance criteria
- Vague or ambiguous requirements
- Missing edge cases
- Unclear dependencies
- Unstated assumptions
- Performance/scale requirements
- Security/privacy concerns
- Error handling scenarios

**Red flags**:
- "Should work well" → How well? Metrics?
- "Users can..." → All users? Which roles?
- "Fast response" → How fast? Under what load?
- "Handle errors" → Which errors? How?
- "Secure" → What security model? Auth? Encryption?

### 2. Question Generation

Creates structured questions in priority order:

**Critical** (must answer before planning):
- Missing core functionality
- Undefined data models
- Unclear user flows
- Security requirements

**Important** (affects architecture):
- Performance targets
- Scale requirements
- Integration points
- Technology constraints

**Nice-to-have** (refinements):
- UI/UX preferences
- Optional features
- Future extensibility

### 3. Interactive Q&A

Presents questions one at a time:
```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Clarification 1/15 [Critical]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Requirement: "Users can create photo albums"

Question: How many photos can be in a single album?

Options:
a) Unlimited
b) Fixed limit (specify)
c) User-configurable limit
d) Skip this question

Your answer: _
```

### 4. Document Answers

Adds "Clarifications" section to specification:
```markdown
## Clarifications

### Albums and Photos
**Q**: How many photos can be in a single album?
**A**: Up to 1000 photos per album. Show warning at 900.

**Q**: Can albums contain other albums (nesting)?
**A**: No. Flat structure only. This was stated in requirements.

**Q**: What happens when user drags album?
**A**: Album reorders in list. Position is persisted.

### Performance
**Q**: How many albums should system support?
**A**: Target: 10,000 albums per user. Load first 100, lazy load rest.

### Security
**Q**: Are photos public or private?
**A**: Private by default. No sharing in MVP.
```

---

## Question Categories

### Data Model Clarifications
- Schema details
- Relationships
- Constraints
- Validation rules
- Data types

### User Experience Clarifications
- Interaction patterns
- Feedback mechanisms
- Error messages
- Loading states
- Empty states

### Performance Clarifications
- Response time targets
- Concurrent users
- Data volume
- Query complexity
- Caching strategy

### Security Clarifications
- Authentication method
- Authorization rules
- Data encryption
- Privacy requirements
- Audit logging

### Edge Cases
- Boundary conditions
- Error scenarios
- Race conditions
- Network failures
- Data corruption

---

## Example Session

```bash
v.clarify

# Analyzing specification...
# Found 15 areas needing clarification

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Clarification 1/15 [Critical]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

User Story: US-001 - Create Photo Albums
Requirement: "Albums are grouped by date"

Question: How are albums grouped by date?

a) Automatically by photo capture date
b) User selects date when creating album
c) User can assign/change date later
d) Group by creation date of album

Your answer: b

Follow-up: What format for the date?

a) Year only (2024)
b) Month and year (Jan 2024)
c) Specific date (2024-01-15)
d) Date range (2024-01-01 to 2024-01-31)

Your answer: b

✅ Documented: Albums have user-assigned Month/Year

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Clarification 2/15 [Critical]
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

User Story: US-002 - Drag and Drop Reordering
Requirement: "Drag and drop on main page"

Question: What happens when user drops album?

a) Albums reorder immediately
b) Show "Save" button to confirm
c) Auto-save with undo option
d) Preview position before confirming

Your answer: a

Question: Is the new order persisted?

Your answer: Yes, save to database immediately

✅ Documented: Drag-drop reorders and auto-saves

... (continues for all 15 questions) ...

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
✅ Clarification Complete
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Answered: 15/15 questions
Critical: 5
Important: 7
Nice-to-have: 3

Updated: .specify/specs/003-photo-organizer/spec.md

Next steps:
- Review clarifications in spec.md
- Run v.plan to create implementation plan
- Clarifications will guide technical decisions
```

---

## Integration with Other Commands

**Before v.clarify**:
- Must have `v.specify` (specification exists)

**After v.clarify**:
- Run `v.plan` with confidence
- Run `v.analyze` to verify consistency
- Technical decisions informed by clarifications

**Skip if**:
- Spike/prototype (exploratory work)
- Requirements are already crystal clear
- Time-boxed experiment

---

## Clarification Strategies

### Sequential Coverage
Questions organized to ensure complete coverage:
1. Core functionality first
2. Data model next
3. User interactions
4. Performance/scale
5. Security/privacy
6. Edge cases
7. Future extensibility

### Assumption Surfacing
Identifies hidden assumptions:
- "It should be obvious..." → Make explicit
- "Users will know..." → Define guidance
- "Like other apps..." → Specify which patterns
- "Standard practice..." → Document the standard

### Edge Case Discovery
Explores boundaries:
- What if no data?
- What if too much data?
- What if network fails?
- What if user cancels mid-operation?
- What if two users edit simultaneously?

---

## Output Format

### In Spec-Kit Mode
Adds section to spec.md:
```markdown
## Clarifications

Recorded during clarification session on 2025-11-04

### Category: Data Model
[Q&A pairs]

### Category: User Experience
[Q&A pairs]

### Category: Performance
[Q&A pairs]

### Category: Security
[Q&A pairs]

### Category: Edge Cases
[Q&A pairs]
```

### In Standalone Mode
Adds to prd.md:
```markdown
## Clarifications

Key decisions made during requirements refinement:

- Albums: Max 1000 photos, no nesting
- Drag-drop: Auto-save, no undo
- Performance: Support 10k albums per user
- Security: Private by default, no sharing in MVP
```

---

## Result

- All ambiguities resolved
- Edge cases identified
- Performance targets defined
- Security requirements clear
- Implementation plan can be created with confidence
- Reduced downstream rework

## Notes

- Structured approach more thorough than ad-hoc
- Records decisions for future reference
- Prevents "we didn't think of that" moments
- Critical for team projects
- Optional but highly recommended
- Can clarify incrementally (run multiple times)
