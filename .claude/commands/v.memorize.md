# v.memorize — Session Consolidation (Task‑Indexed Archiving)
**Purpose**: Persist today’s work to `progress.md`, keep live files slim, and make history discoverable by **Task ID** (e.g., T-010).

This command goal is that humans and AIs can jump straight to `archive/T-XXX.md` for deep context.

### Update progress.md


**Full Prompt**:
```
Update @memory/progress.md with the current session's accomplishments.

Session Summary:
- **Date**: [YYYY-MM-DD]
- **Phase**: [Development/Testing/Production/Documentation]
- **Main Tasks**: [LIST_OF_TASKS]
- **Outcomes**: [MEASURABLE_RESULTS]
- **Blockers**: [ANY_ISSUES_FOUND]
- **Next Steps**: [WHAT_REMAINS]

Updates Needed:
1. Update "Last Updated" date at top
2. Update "Current Phase" if changed
3. Update "Active Components" versions if scripts modified
4. Update "Managed Security Lists" counts if data changed
5. Update "Performance Metrics" if benchmarks changed
6. Add decision log entry if architectural change
7. Add completed work to "Completed Major Work" section
8. Update "Known Issues" if new issues or resolutions
9. Update "Next Steps" section with remaining work


## IMPORTANT
- Each progress entry is tagged with a **Task ID** `[T-xxx]` (optional Step IDs `[S-xxxx]`).
- When details grow, **consolidate by task** into `ai/memory/archive/T-xxx.md`.
- In `progress.md`, keep a **one‑line pointer** per task instead of long text.
- Auto‑pull the task title and acceptance criteria anchor from `plan.md` and record them in the archive file.
- Maintain `ai/memory/archive/index.md` (if present).

Requirements:
- Keep "Recent Decisions Log" to last 30 days only
- Archive older entries to memory/archive/
- Maintain concise, factual language
- Include version numbers and file references
- Quantify changes (entry counts, performance, etc.)
```

**Example Usage**:
```
Update @memory/progress.md with the current session's accomplishments.

Session Summary:
- **Date**: 2025-10-18
- **Phase**: Documentation
- **Main Tasks**: Document WAF integration in architecture diagrams, fix GitHub Mermaid compatibility
- **Outcomes**: Updated docs/DIAGRAMs.md v1.1, discovered GitHub Mermaid parser rules
- **Blockers**: None - resolved all rendering issues
- **Next Steps**: None - documentation complete

Updates Needed:
1. Update "Last Updated" to 2025-10-18
2. Add "Documentation: WAF Integration in Architecture Diagrams" to decision log
3. Add Lesson #7 about GitHub Mermaid Parser Compatibility
4. Mark documentation tasks as complete in "Completed Major Work"
5. Confirm no new known issues
```

---

## Archive File Schema
If `ai/memory/archive/T-xxx.md` is new, start with YAML front matter:
```yaml
---
id: T-010
title: Core Structure & Authentication
status: In_Progress
last_update: 2025-11-04
tags: []
---
# T-010 — Core Structure & Authentication
Origin: plan.md
```
Append dated sections for each session:
```markdown
## 2025-11-04
Task: T-010 — Core Structure & Authentication
Status: Done|In_Progress|Blocked
Steps: [S-0101, S-0102]   # optional
Summary: <2–4 lines>
Difficulties: <bullets>
Solutions: <bullets>
Evidence: commits [abc123..def456], PR #42, tests green
Links: plan.md#T-010 (Acceptance), progress.md
```

---

## Steps
1. Collect session notes in `progress.md` since last run; require Task IDs. Infer if missing; abort if ambiguous.
2. Group by Task ID; open/create `archive/T-xxx.md`; write YAML header if missing; append dated section.
3. Replace verbose progress for each task with a **one‑line pointer**:

   `- [T-010] Done — details: ai/memory/archive/T-010.md#2025-11-04`

4. Update `archive/index.md` (if present) with ID, Title, Last Update, Status, File path.
5. If a task status flips to **Done**, add an Unreleased entry in `CHANGELOG.md`.

---

## Result
- `progress.md` is compact and navigable.
- `archive/T-xxx.md` contains rich, structured history.
- Optional `archive/index.md` lists tasks with last update and status.
- `plan.md` and `progress.md` remain small, with durable pointers.

---

## Notes
- Run `v.checkpoint` after archiving to validate and commit.
- For merging pre-framework notes, use `v.initmemory` before running this command.

---

### Update CHANGELOG.md

**Full Prompt**:
```
Add an entry to @docs/CHANGELOG.md for the recent changes.

Version: [X.Y.Z]
Date: [YYYY-MM-DD]
Type: [Added/Changed/Fixed/Removed/Security]

Changes:
- [CHANGE_1] [affected_file.sh or component]
- [CHANGE_2] [affected_file.sh:function]
- [CHANGE_3] [affected_file.sh:line-range]

Template:
## [X.Y.Z] - YYYY-MM-DD

### [Type]
- [Change description with technical details] [file reference]

Requirements:
- Insert at the top of the changelog (most recent first)
- Use clear, technical language
- Include file/function references
- Quantify impact where possible (performance, scale, etc.)
- Link to issue numbers if applicable
- Follow existing format (markdown, bullets, references)
```

**Example Usage**:
```
Add an entry to @docs/CHANGELOG.md for the recent changes.

Version: 1.6.7
Date: 2025-10-17
Type: Fixed

Changes:
- Fixed AWK compatibility issue in WAF IP extraction (GNU Awk 3.1.7 doesn't support {1,3} quantifiers) [syncWhitelist.sh:extract_waf_blacklist]
- Changed regex pattern from {1,3} to + quantifiers for backwards compatibility [syncWhitelist.sh:375]
- Successfully merged ~1,086 WAF-banned IPs into blacklist_generated.lst [syncWhitelist.sh]

Template:
## [1.6.7] - 2025-10-17

### Fixed
- Fixed AWK compatibility issue in WAF IP extraction - GNU Awk 3.1.7 (CentOS 6) doesn't support `{1,3}` range quantifiers, changed to simple `+` quantifiers [syncWhitelist.sh:extract_waf_blacklist()]
- Successfully merged ~1,086 WAF-banned IPs from /etc/nginx/banned_ips.conf into blacklist_generated.lst [syncWhitelist.sh]
- Total blacklist now ~1,224 entries (138 manual + 1,086 WAF auto-bans) [system-wide impact]
```

---

