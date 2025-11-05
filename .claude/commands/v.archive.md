# v.archive — Rotate Bulky Details to Archive

**Purpose**: Keep progress.md and plan.md slim by offloading long sections.

---

## Steps
1. Detect sections older than 30–60 days or >500 lines total.
2. Move verbose content into _ai/memory/archive/<ID-or-date>.md.
3. Leave a one-line pointer in the live file.
4. Update progress.md 'Pointers' section.

---

## Result
- Live files stay short.
- Details preserved with stable links.
