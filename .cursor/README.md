# Cursor Custom Commands

This directory contains Markdown proxy files for Cursor AI custom commands.

## Usage

When using [Cursor](https://cursor.com), reference these commands in chat:

```
@.cursor/commands/v.onboard.md
@.cursor/commands/v.specify.md
@.cursor/commands/v.plan.md
@.cursor/commands/v.next.md
@.cursor/commands/v.do.md
# ... and 23 more
```

Or use them as custom commands if Cursor supports that feature.

## How It Works

Each `.md` file in `commands/` acts as a **proxy** to the actual command specification in `.claude/commands/`.

**Example**: `commands/v.onboard.md` → `.claude/commands/v.onboard.md`

The proxy file contains:
- Command name and purpose
- Reference to full specification

This approach avoids duplication and keeps maintenance simple.

## Structure

```
.cursor/
├── README.md
└── commands/
    ├── v.onboard.md
    ├── v.specify.md
    ├── v.plan.md
    └── ... (28 total)
```

## Adding New Commands

When adding a new `v.*` command:

1. Create `.claude/commands/v.newcommand.md` with full specification
2. Run generation script to create proxy
3. Reference in Cursor using `@.cursor/commands/v.newcommand.md`

## Alternative: .cursorrules

Cursor also supports `.cursorrules` file. See `.github/copilot-instructions.md` for a comprehensive list of all commands that can be referenced.

## Documentation

For complete command documentation, see:
- **Command specs**: `.claude/commands/v.*.md`
- **Quick reference**: `_ai/QUICKREF.md`
- **Operating manual**: `CLAUDE.md`
- **Copilot/Cursor instructions**: `.github/copilot-instructions.md`

## Version

**Framework**: v2.5.0  
**Commands**: 28  
**Format**: Markdown  
