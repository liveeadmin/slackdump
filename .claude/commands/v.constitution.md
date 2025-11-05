# v.constitution — Create Project Constitution

**Purpose**: Create or update project governing principles that guide all development decisions. Works in both Standalone and Spec-Kit modes.

---

## Usage

```bash
v.constitution [description]
```

**Examples**:
```bash
v.constitution Create principles focused on code quality, testing standards, and performance

v.constitution Update constitution with new security requirements
```

---

## Behavior by Mode

### Spec-Kit Mode (if `.specify/` exists)
Creates/updates `.specify/memory/constitution.md` with:
- Core development principles (numbered sections)
- Testing standards (TDD, coverage requirements)
- Architecture patterns (library-first, modularity)
- Quality gates (code review, acceptance criteria)
- Governance rules (amendment process)

### Standalone Mode
Creates/updates `_ai/memory/constitution.md`:
- Project-specific principles and rules
- Testing standards (test-first, coverage targets)
- Architecture patterns (modularity, naming)
- Quality requirements (file size, code style)
- Security and performance standards

**Note**: CLAUDE.md contains framework rules. Constitution.md contains project-specific rules.

---

## Constitution Template Structure

```markdown
# [PROJECT_NAME] Constitution

## Core Principles

### I. [PRINCIPLE_1_NAME]
[Description and rules]

### II. [PRINCIPLE_2_NAME]
[Description and rules]

## Quality Standards
- Code quality requirements
- Testing requirements
- Security requirements
- Performance requirements

## Governance
- Amendment process
- Approval requirements
- Version tracking
```

---

## Steps

1. **Detect Mode**
   - Check if `.specify/memory/` exists
   - Determine target file location

2. **Analyze Current Practices**
   - Read existing code patterns
   - Review CLAUDE.md principles
   - Check for existing test patterns
   - Identify architecture decisions

3. **Generate Constitution**
   - Extract core principles
   - Format as numbered sections
   - Include rationale for each principle
   - Add governance rules

4. **Interactive Refinement**
   - Present draft constitution
   - Ask clarifying questions
   - Refine based on user feedback
   - Finalize and save

5. **Update References**
   - Update CLAUDE.md to reference constitution (if Spec-Kit mode)
   - Update documentation with constitution location
   - Link to constitution in relevant commands

---

## Common Principles to Include

### Code Quality
- Clarity over cleverness
- Single responsibility
- Minimal dependencies
- File size limits (600 lines)

### Testing
- Test-first development (TDD)
- Fast, hermetic tests
- Coverage requirements
- Integration test strategy

### Architecture
- Library-first approach
- Modular design
- Clear interfaces
- Separation of concerns

### Security
- No secrets in code
- Input validation
- Safe error handling
- Audit trail

### Process
- Tasks in order
- Small commits
- Code review requirements
- Documentation standards

---

## Integration with Commands

After constitution exists:
- `v.do` checks constitution compliance
- `v.review` validates against principles
- `v.plan` references constitution patterns
- `v.checkpoint` verifies governance rules

---

## Example Usage

```bash
# Create initial constitution
v.constitution Create principles for a REST API project focused on reliability and observability

# Agent prompts:
# "What testing strategy do you prefer? (TDD, test-after, mixed)"
# "What are your file size limits? (default: 600 lines)"
# "Should libraries be mandatory-first or recommended?"
# etc.

# Result: .specify/memory/constitution.md created (or CLAUDE.md updated)
```

---

## Result

- Constitution file created/updated
- Principles clearly documented
- Governance rules established
- Commands aware of constitution
- Team has shared understanding

## Notes

- Constitution should be **immutable** once established
- Amendments require documentation and rationale
- Spec-Kit mode creates formal constitution.md
- Standalone mode enhances CLAUDE.md
- Both approaches enforce same principles
