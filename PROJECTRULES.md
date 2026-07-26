# Project-Specific Rules — AI Framework Template

**Version**: 1.0.0 | **Last Updated**: 2025-11-05  
**Project**: VibeLogic AI Framework (ai_projectmodel)

---

## Purpose

This file contains project-specific rules and workflows that apply ONLY to this project (the AI framework template itself). Other projects using this template should have their own PROJECTRULES.md.

**Note**: CLAUDE.md checks for this file and loads it if present.

---

## Project Context

This is the **VibeLogic AI Framework** repository itself — the meta-project that provides the `v.*` commands and workflow templates for other projects.

**Key Distinction**:
- This project **creates** the framework
- Other projects **use** the framework

---

## Special Workflows for Framework Development

### Adding New Commands

When adding a new `v.command` to the framework:

1. **Create command specification** in `.claude/commands/v.command.md`
2. **Create proxy files** for ALL supported agents:
   - `.gemini/commands/v.command.toml` (Gemini Code Assist)
   - `.cursor/commands/v.command.md` (Cursor)
   - Update `.github/copilot-instructions.md` (GitHub Copilot)
3. **Update documentation**:
   - Add to command list in CLAUDE.md (if major command)
   - Update `.github/copilot-instructions.md` command list
   - Add to CHANGELOG.md
4. **Test command** with at least one agent

### Command Proxy Format

**Gemini** (`.gemini/commands/v.command.toml`):
```toml
description = "Brief description of command"
prompt = """
Load and execute the instructions from @.claude/commands/v.command.md
"""
```

**Cursor** (`.cursor/commands/v.command.md`):
```markdown
Load and execute the instructions from @.claude/commands/v.command.md
```

**Copilot** (`.github/copilot-instructions.md`):
```markdown
- **@.claude/commands/v.command.md** — Description
```

**Claude** (`.claude/commands/v.command.md`):
- Full command specification (source of truth)

---

## Current Command Status

### ✅ Fully Implemented (All Agents)

Commands with proxy files for Gemini, Cursor, and Copilot documentation:
- v.specify, v.plan, v.tasks, v.do, v.implement
- v.next, v.resume, v.checkpoint, v.memorize, v.archive
- v.review, v.analyze, v.clarify, v.checklist, v.testsync
- v.onboard, v.constitution, v.initproject, v.initmemory
- v.help, v.mode, v.feature, v.speckit, v.whatif
- v.shrink, v.syncdocs, v.createprd, v.docdiagrams

### ✅ Recently Added (2025-11-05)

**v.doall** — Fast vibe coding execution
- ✅ Claude: `.claude/commands/v.doall.md`
- ✅ Gemini: `.gemini/commands/v.doall.toml`
- ✅ Cursor: `.cursor/commands/v.doall.md`
- ✅ Copilot: `.github/copilot-instructions.md` (updated)

**v.debug** — Structured bug investigation
- ✅ Claude: `.claude/commands/v.debug.md`
- ✅ Gemini: `.gemini/commands/v.debug.toml`
- ✅ Cursor: `.cursor/commands/v.debug.md`
- ✅ Copilot: `.github/copilot-instructions.md` (updated)

---

## Multi-Agent Testing Protocol

When testing framework commands:

1. **Primary agent** (Claude): Test full command execution
2. **Secondary agents**: Verify proxy loading works
3. **Document** any agent-specific quirks in command file
4. **Cross-reference** ensure all agents can access command

---

## Documentation Synchronization

Keep these files in sync:
- `CLAUDE.md` — Authoritative framework documentation
- `GEMINI.md` — Agent-specific notes (if any)
- `.github/copilot-instructions.md` — Command reference for Copilot
- `.cursor/README.md` — Cursor-specific guidance
- `CHANGELOG.md` — Version history

---

## File Size Limits (Framework Files)

Unlike user projects (600-line limit), framework documentation can be larger:
- Command specs: Up to 1000 lines (comprehensive examples)
- CLAUDE.md: Up to 2000 lines (complete manual)
- Other docs: 600-line limit still applies

Rationale: Framework docs are reference material, not runtime code.

---

## Version Management

Framework follows semantic versioning:
- **Major** (X.0.0): Breaking changes to command behavior
- **Minor** (0.X.0): New commands, new features
- **Patch** (0.0.X): Bug fixes, doc improvements

Current version: 2.7.0 (added v.doall, v.debug)

---

## Testing Checklist for New Commands

Before considering a new command "complete":

- [ ] Command spec created in `.claude/commands/`
- [ ] Gemini TOML proxy created in `.gemini/commands/`
- [ ] Cursor MD proxy created in `.cursor/commands/`
- [ ] Copilot instructions updated in `.github/copilot-instructions.md`
- [ ] Command tested with at least one agent
- [ ] Documentation updated (CLAUDE.md if major)
- [ ] CHANGELOG.md entry added
- [ ] Version number incremented (if releasing)

---

## Known Limitations

### Agent-Specific Behaviors

**Claude Code (primary)**:
- Full access to all commands via `.claude/commands/`
- Can read multi-line command specs
- Best for framework development

**Gemini Code Assist**:
- Uses TOML proxy files
- Loads Claude specs via `@` reference
- Works well for command execution

**Cursor**:
- Uses `.cursor/commands/` directory
- Simpler proxy format (just load instruction)
- May need explicit file references

**GitHub Copilot**:
- No custom command support built-in
- Uses `.github/copilot-instructions.md` for context
- Commands listed as documentation references

---

## Contributing to Framework

When extending the framework:

1. **Discuss** major changes in progress.md or GitHub issues
2. **Follow** existing command patterns and conventions
3. **Test** with multiple agents when possible
4. **Document** in both command spec and CLAUDE.md
5. **Update** all proxy files for multi-agent support

---

## Recent Completions

### ✅ v.doall proxy files (Completed 2025-11-05)
- [x] Created `.cursor/commands/v.doall.md`
- [x] Updated `.github/copilot-instructions.md` with v.doall entry
- [x] Created Gemini TOML proxy
- [x] Verified all agents can access command

### ✅ v.debug proxy files (Completed 2025-11-05)
- [x] Created `.claude/commands/v.debug.md` (500+ lines)
- [x] Created `.gemini/commands/v.debug.toml`
- [x] Created `.cursor/commands/v.debug.md`
- [x] Updated `.github/copilot-instructions.md`
- [x] Added workflows to CLAUDE.md
- [x] Updated framework version to 2.7.0

---

## Notes

- This PROJECTRULES.md is specific to the framework repository
- Projects using the framework will have their own PROJECTRULES.md
- Keep this file updated when adding commands or changing workflows
- Review this file when onboarding to framework development
