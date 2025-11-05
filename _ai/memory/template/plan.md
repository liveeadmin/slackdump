# plan.md — Phases, Tasks, Steps

ID scheme:
- Phase: PH-01, PH-02, ...
- Task:  T-010, T-020, ...
- Step:  S-0101, S-0102, ... (belongs to a task)

Each task defines acceptance criteria and a brief test plan. Keep descriptions short; put long specs in _ai/memory/archive/<ID>.md and link.

## Current Phase
PH-01 — MVP scaffolding

## Task Queue (execute in order)
1. T-010 — Bootstrap repo structure
   - Steps:
     - S-0101 — Create folders and baseline tooling
     - S-0102 — CI smoke tests
   - Accept: CI green, lint ok
   - Details: see _ai/memory/archive/T-010.md

2. T-020 — Core module A
   - Steps:
     - S-0201 — Model + unit tests
     - S-0202 — Service API + tests
   - Accept: endpoints tested; perf budget met
   - Details: _ai/memory/archive/T-020.md

## Completed
- [ ] T-010
- [ ] T-020
