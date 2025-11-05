# Gemini CLI Custom Commands

This directory contains TOML proxy files for Gemini CLI custom slash commands.

## Usage

When using [Gemini CLI](https://github.com/google-gemini/gemini-cli), these commands will be available as slash commands:

```bash
/v.onboard      # Reverse engineer existing project
/v.specify      # Define requirements
/v.plan         # Create implementation plan
/v.next         # Show next task
/v.do           # Execute task
# ... and 23 more commands
```

## How It Works

Each `.toml` file in `commands/` acts as a **proxy** to the actual command specification in `.claude/commands/`.

**Example**: `commands/v.onboard.toml` → `.claude/commands/v.onboard.md`

The TOML file contains:
- Command description
- Reference to full specification

This approach avoids duplication and keeps maintenance simple.

## Structure

```
.gemini/
├── README.md
└── commands/
    ├── v.onboard.toml
    ├── v.specify.toml
    ├── v.plan.toml
    └── ... (28 total)
```

## Adding New Commands

When adding a new `v.*` command:

1. Create `.claude/commands/v.newcommand.md` with full specification
2. Run generation script to create proxy
3. Gemini CLI will auto-discover the new slash command

## Documentation

For complete command documentation, see:
- **Command specs**: `.claude/commands/v.*.md`
- **Quick reference**: `_ai/QUICKREF.md`
- **Operating manual**: `CLAUDE.md`

## Version

**Framework**: v2.5.0  
**Commands**: 28  
**Format**: Gemini CLI TOML  
