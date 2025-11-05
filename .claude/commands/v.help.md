# v.help — Interactive Help and Guidance

**Purpose**: Guide users through the framework with interactive help, command suggestions, and workflow recommendations based on current project state.

---

## Usage

```bash
v.help [topic]
```

**Topics**:
- No arguments: Interactive help menu
- `commands` — List all commands with descriptions
- `workflow` — Show recommended workflow for current state
- `next` — Suggest what to do next
- `setup` — Initial project setup guidance
- `modes` — Explain Standalone vs Spec-Kit modes
- `constitution` — Help with creating project constitution
- `<command>` — Detailed help for specific command

**Examples**:
```bash
v.help                    # Interactive menu
v.help workflow           # Show workflow guidance
v.help next               # What should I do next?
v.help v.specify          # Help with v.specify command
v.help setup              # Initial setup guide
```

---

## Interactive Help Menu

When called without arguments:

```bash
v.help

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📚 Vibe-Coding Framework Help
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Current Project Status:
├─ Mode: Standalone
├─ Constitution: Not found
├─ Specification: Not found
├─ Plan: Not found
└─ Tasks: Not found

What would you like to do?

1. 🚀 Start a new project
2. 📝 Learn about workflows
3. 📋 See all commands
4. 🔍 Understand modes (Standalone vs Spec-Kit)
5. 💡 Get suggestion for next step
6. 📖 Read documentation
7. 🔧 Troubleshooting

Enter number (1-7) or 'q' to quit: _
```

---

## Help by Topic

### 1. Starting a New Project (`v.help setup`)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🚀 Starting a New Project
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Choose your path:

┌─────────────────────────────────────┐
│ Quick Start (Minimal Setup)        │
└─────────────────────────────────────┘
1. v.specify "Build a [description]"
2. v.plan "Tech stack details"
3. v.tasks
4. v.implement

Time: ~5 minutes to start coding
Best for: Solo projects, prototypes

┌─────────────────────────────────────┐
│ Quality-Focused (Recommended)       │
└─────────────────────────────────────┘
1. v.constitution (define project rules)
2. v.specify "Build a [description]"
3. v.clarify (resolve ambiguities)
4. v.checklist spec (validate quality)
5. v.plan "Tech stack details"
6. v.tasks
7. v.analyze (check consistency)
8. v.implement

Time: ~15-20 minutes to start coding
Best for: Team projects, production code

┌─────────────────────────────────────┐
│ Spec-Kit Mode (Full Governance)    │
└─────────────────────────────────────┘
1. v.speckit init
2. v.constitution
3. /speckit.specify or v.specify
4. /speckit.clarify or v.clarify
5. /speckit.plan or v.plan
6. /speckit.tasks or v.tasks
7. /speckit.implement or v.implement

Time: ~20-30 minutes to start coding
Best for: Large teams, governed projects

What would you like to do? (1/2/3): _
```

### 2. Understanding Workflows (`v.help workflow`)

Analyzes current project state and recommends next steps:

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📝 Workflow Guidance
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Current State Analysis:
✅ Specification exists (prd.md)
✅ Plan exists (plan.md)
❌ No tasks breakdown found
❌ No constitution found

Recommended Next Steps:

1. Create Constitution (Optional but Recommended)
   → v.constitution
   Define project-specific rules and standards

2. Generate Task Breakdown
   → v.tasks
   Break plan into actionable tasks

3. Validate Consistency
   → v.analyze
   Check for gaps and conflicts

4. Execute Implementation
   → v.implement (automated)
   or v.next → v.do (manual control)

Would you like me to run 'v.tasks' now? (y/n): _
```

### 3. All Commands (`v.help commands`)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📋 Available Commands
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

🔷 Primary Workflow
  v.constitution     Create project governing principles
  v.specify          Define requirements (WHAT/WHY)
  v.plan             Create implementation plan (HOW)
  v.tasks            Generate task breakdown
  v.implement        Execute all tasks automatically

🔷 Session Management
  v.next             Show next task
  v.do               Execute current task
  v.resume           Resume interrupted work
  v.checkpoint       Stabilize and commit

🔷 Quality & Validation
  v.clarify          Clarify underspecified requirements
  v.analyze          Cross-artifact consistency analysis
  v.checklist        Generate quality validation checklists
  v.review           Code review and validation
  v.shrink           Enforce file size limits
  v.testsync         Sync tests with code

🔷 Memory Management
  v.memorize         Archive completed work
  v.archive          Manual archiving
  v.initmemory       Import legacy data

🔷 Utility
  v.whatif           Evaluate ideas
  v.syncdocs         Update documentation
  v.help             This help system

🔷 Integration
  v.speckit          Spec-Kit bridge commands
    init             Convert to Spec-Kit mode
    status           Show active mode
    constitution     Generate constitution
    sync             Bidirectional sync

For detailed help on any command: v.help <command>
For workflow guidance: v.help workflow
```

### 4. Understanding Modes (`v.help modes`)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔍 Standalone vs Spec-Kit Modes
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

┌─────────────────────────────────────┐
│ Standalone Mode (Lightweight)      │
└─────────────────────────────────────┘

Structure:
  _ai/memory/
    ├─ constitution.md
    ├─ prd.md
    ├─ plan.md
    ├─ progress.md
    └─ resume.md

Best For:
  ✓ Solo developers
  ✓ Small projects
  ✓ Rapid prototyping
  ✓ Learning/experimenting

Commands: v.* only

┌─────────────────────────────────────┐
│ Spec-Kit Mode (Structured)         │
└─────────────────────────────────────┘

Structure:
  .specify/
    ├─ memory/constitution.md
    └─ specs/NNN-feature/
       ├─ spec.md
       ├─ plan.md
       └─ tasks.md

Best For:
  ✓ Team projects
  ✓ Governed projects
  ✓ Client work
  ✓ Complex features

Commands: Both v.* and /speckit.*

┌─────────────────────────────────────┐
│ Current Mode                        │
└─────────────────────────────────────┘

Your project: Standalone Mode

To check mode: v.speckit status
To upgrade: v.speckit init

Need more help? (y/n): _
```

### 5. What's Next? (`v.help next`)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
💡 What Should I Do Next?
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Analyzing project state...

Current Status:
✅ Specification defined
✅ Plan created
✅ Tasks generated
🔄 Implementation in progress (Task 3 of 12)

Next Action:
→ Continue implementation with v.do

Details:
  Current Task: T-003 "Create Album Service"
  Files: src/services/album-service.js
  Status: In progress (50% complete)

Options:
1. Resume current task    → v.resume
2. Check what's next      → v.next
3. Execute next step      → v.do
4. Checkpoint progress    → v.checkpoint

Recommended: v.resume (you have work in progress)

Execute now? (y/n): _
```

### 6. Command-Specific Help (`v.help <command>`)

```bash
v.help v.specify

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
📖 Help: v.specify
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Purpose:
  Define requirements and specifications.
  Focus on WHAT you're building and WHY,
  not the tech stack (that comes in v.plan).

Usage:
  v.specify [feature description]

Example:
  v.specify Build a photo organizer with 
  drag-drop albums grouped by date

What It Does:
  1. Creates specification document
  2. Generates user stories
  3. Defines acceptance criteria
  4. Identifies open questions

Output:
  Standalone: _ai/memory/prd.md
  Spec-Kit: .specify/specs/NNN-*/spec.md

Next Steps:
  After v.specify:
  → v.clarify (optional, resolve ambiguities)
  → v.checklist spec (optional, validate quality)
  → v.plan (define tech stack)

See Also:
  v.help v.plan
  v.help v.clarify
  v.help workflow

Try it now? (y/n): _
```

---

## Context-Aware Suggestions

Help system analyzes project state to give relevant advice:

### No Files Exist
```
It looks like you're just starting!

Try: v.help setup
```

### Spec Exists, No Plan
```
You have a specification but no implementation plan.

Next step: v.plan "Your tech stack description"
```

### Plan Exists, No Tasks
```
You have a plan but haven't broken it into tasks.

Next step: v.tasks
```

### Tasks Exist, Not Started
```
Tasks are ready for implementation!

Options:
- Automated: v.implement
- Manual: v.next → v.do
```

### Work In Progress (resume.md exists)
```
You have unfinished work!

Next step: v.resume
```

### Constitution Missing
```
Consider creating a constitution to define project standards:

Try: v.constitution
```

---

## Troubleshooting (`v.help troubleshooting`)

```
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
🔧 Troubleshooting
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

Common Issues:

1. "I don't know which mode I'm in"
   → v.speckit status

2. "Commands can't find my files"
   → Check mode detection
   → Verify file structure

3. "Lost track of what I was doing"
   → cat _ai/memory/resume.md
   → v.next

4. "File size exceeded 600 lines"
   → v.shrink

5. "Need to sync between modes"
   → v.speckit sync

6. "Tests not passing"
   → v.testsync

7. "Want to start over"
   → v.initproject (regenerate structure)

8. "Legacy project migration"
   → v.initmemory

More help: v.help <topic>
Documentation: README.md, CLAUDE.md
```

---

## Quick Reference Card

```bash
v.help quick

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
⚡ Quick Reference
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

New Project:
  v.specify → v.plan → v.tasks → v.implement

Resume Work:
  v.resume or v.next → v.do

Quality Check:
  v.clarify → v.analyze → v.checklist

Stabilize:
  v.checkpoint

Get Help:
  v.help [topic]

Check Mode:
  v.speckit status

Full Guide:
  v.help workflow
```

---

## Integration with Other Commands

Help system can execute commands directly:

```bash
v.help next

# Suggests: v.do

Execute now? (y/n): y

# Runs v.do automatically
```

---

## Help Topics Index

- `setup` — Initial project setup
- `workflow` — Workflow guidance
- `commands` — All commands list
- `modes` — Standalone vs Spec-Kit
- `constitution` — Creating project rules
- `next` — What to do next
- `troubleshooting` — Common issues
- `quick` — Quick reference card
- `<command>` — Specific command help

---

## Result

- Context-aware guidance
- Interactive help menu
- Workflow recommendations
- Command execution shortcuts
- Troubleshooting assistance
- Quick reference

## Notes

- Help system analyzes current project state
- Suggestions are context-sensitive
- Can execute suggested commands directly
- Useful for onboarding new team members
- Reduces need to read full documentation
- Updates based on project progress
