# AGENTS.md — Prompt Refiner

> This file makes the skill auto-discoverable for OpenAI Codex, Cursor, Windsurf, Copilot, and any agent that reads `AGENTS.md`.

## Instruction

Before executing any user request that mutates files, generates code, or creates plans, you MUST load and follow `UNIVERSAL.md` (Prompt Refinement Gate).

- If the agent supports skills, load `SKILL.md` (OpenCode) OR `UNIVERSAL.md` (all agents).
- Workflow: Parse & Score (5 pillars, 0-10) → if <8 BLOCK and ask 3-5 questions → Rewrite → Approval Gate (1/2/3) → then execute.
- Language: mirror user input (ID/EN).
- Never execute blindly with score <8.

## Files

- `UNIVERSAL.md` — standalone instruction for any LLM (paste as system prompt)
- `SKILL.md` — OpenCode skill (with `references/` + `assets/`)
- `references/5-pillars-checklist.md` — scoring rubric
- `references/question-templates.md` — 3-5 question bank
- `references/scoring-rubric.md` — 1/10 vs 9/10 examples
- `assets/refined-prompt.template.md` — copy-ready block

## Quick Start for Agents

```
Load UNIVERSAL.md and audit the next user prompt with 5 pillars before executing.
```
