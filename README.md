# Luna Agentic Workflow v2

Luna Agentic Workflow v2 is a reusable, tool-aware workflow for reliable agentic work.

It is derived from the core ideas in `jha0313/agentic-workflow-playbook`, but redesigned for a **ChatGPT + GitHub** environment and deliberately separated from any single coding agent or CLI.

## Important: this repository is reusable

You do **not** create a new Luna workflow repository for every new program.

This repository is the reusable **workflow/methodology layer**. A new software project normally needs only:

1. its own GitHub repository;
2. its own project rules/context;
3. the Luna workflow applied to that repository.

The existing `eo4992-boop/gpt` repository remains the Luna Chat Coder runtime/continuity layer. This repository defines the broader workflow methodology. They can later be integrated if the workflow proves useful in practice.

## Workflow

```text
0 Context
   ↓
1 Intent
   ↓
2 Specification
   ↓
3 Plan / Decompose
   ↓
4 Implement
   ↓
5-A Gate ──failure──→ bounded implementation retry
   ↓
5-B Independent Review ──failure──→ Specification
   ↓
6 Ship & Learn
   ↓
Context update → next loop
```

### Core invariant

> The agent that performs the work must not be the sole judge of whether the work is correct.

5-A is deterministic verification. 5-B is independent judgment. Passing 5-A is necessary, but not sufficient.

## ChatGPT + GitHub binding

- **ChatGPT** is the conversational orchestrator and engineering agent.
- **Sandbox** is the primary implementation workspace when available.
- **GitHub** is the durable source of truth for repository state, history, and handoff.
- **GitHub Actions** are verification/fallback infrastructure, not the default coding environment.
- **Human approval** is required when risk or ambiguity makes automated judgment insufficient.

See `WORKFLOW.md` for the normative workflow and `adapters/chatgpt-github.md` for the environment binding.

## Source analysis

The source playbook describes a six-stage loop, persistent context, bounded retries, decomposition, and a two-layer validation model. Its repository also contains Claude Code session/log artifacts and tooling. This project retains the methodology and removes those environment-specific assumptions.

See:

- `docs/source-analysis.md`
- `docs/claude-code-removal.md`
- `docs/design-rationale.md`
- `templates/project-context.md`
- `templates/task.md`
- `templates/review.md`
