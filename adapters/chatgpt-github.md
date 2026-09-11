# ChatGPT + GitHub Adapter

This adapter binds the host-neutral Luna workflow to the environment used by Luna Chat Coder.

## Roles

### ChatGPT

ChatGPT can perform three functions in one conversation:

- orchestrate the workflow;
- implement the requested change;
- perform a first-pass self-check.

The third function is not the final independent review. A separate review pass, human review, or other evidence must provide 5-B when the task requires it.

### Sandbox

When available, use the sandbox as the primary implementation workspace. Work against the exact repository state that is being changed.

### GitHub

GitHub is the durable source of truth for:

- repository files;
- branches and commits;
- pull requests;
- review discussion;
- CI results and artifacts;
- durable handoff.

### GitHub Actions

Actions are a verification and bounded fallback mechanism. They are not the default place to perform arbitrary development merely because they are available.

## Repository-first procedure

Before implementation:

1. identify the target repository;
2. inspect repository instructions;
3. inspect current branch/PR/commit state;
4. read only the relevant architecture and source files needed for the task;
5. establish the exact baseline;
6. define verification evidence before making claims.

## New project procedure

A new application does **not** require a new Luna workflow repository.

Use this repository as the reusable workflow reference. The application repository should contain its own project-specific context, such as:

```text
AGENTS.md / project instructions
architecture notes
project decisions
verification commands
acceptance criteria
known constraints
```

If a project already has a stronger repository-specific workflow, Luna should adapt to it rather than overwrite it.

## Existing Luna Chat Coder relationship

`eo4992-boop/gpt` and this repository intentionally have different responsibilities:

| Repository | Responsibility |
|---|---|
| `gpt` | Luna Chat Coder runtime/continuity/fallback policy |
| `luna-agentic-workflow` | reusable agentic workflow methodology |
| target application repo | actual product code and project-specific rules |

The workflow can later be integrated into Luna Chat Coder after practical validation.

## Evidence requirements

Report verification with concrete evidence:

- command executed;
- result/status;
- commit or PR reference when relevant;
- limitations or skipped checks.

Do not say "tested" when the test was not actually executed.
