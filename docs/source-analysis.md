# Source Playbook Analysis

Source: `jha0313/agentic-workflow-playbook`.

The source repository contains a README, a normative workflow playbook, a drop-in orchestrator template, a demo prompt, HTML presentation material, large session-log HTML files, and a Python log-to-HTML utility. The repository tree confirms these distinct categories. fileciteturn14file0

## Core methodology retained

The source playbook's durable ideas are:

1. Context is persistent project memory.
2. Intent removes outcome-changing ambiguity before execution.
3. Specification becomes an explicit grading checklist.
4. Work is decomposed into bounded units with dependencies and success criteria.
5. Implementation is separated from validation.
6. Validation has two layers: deterministic 5-A and independent 5-B.
7. 5-A failures use bounded retries.
8. 5-B failures return to specification rather than blindly rewriting code.
9. Improvement feeds lessons back into persistent context.
10. Verification strength should increase with risk.

The source explicitly emphasizes that the worker should not grade its own result and that the workflow is a loop rather than a one-shot prompt. fileciteturn16file0

## Source workflow model

The source describes the loop as:

`0 Context → 1 Intent → 2 Spec → 3 Decompose → 4 Implement → 5 Verify → 6 Improve`.

Its 5-A/5-B distinction is especially important: 5-A is cheap, deterministic verification; 5-B is external final judgment. A 5-A failure retries the affected unit with a bounded limit, while a 5-B failure returns to specification. fileciteturn16file0

## Important adaptation

The source says the playbook is conceptually tool-agnostic, but its repository and examples are operationally centered on Claude Code. The README explicitly presents Claude Code in its startup instructions and its log tooling uses Claude session JSONL paths. fileciteturn15file0

Luna therefore keeps the workflow semantics but changes the execution model:

- no mandatory Claude session;
- no mandatory sub-agent process;
- no Claude filesystem paths;
- no Claude JSONL log format;
- no requirement for a dedicated orchestrator process;
- no assumption that the orchestrator itself cannot execute work;
- no requirement that one decomposition chunk equal one sub-agent session.

The new invariant is narrower and more portable: **execution and final judgment must be separated**, while orchestration and execution may coexist in the same ChatGPT conversation.

## Why this matters for ChatGPT

ChatGPT can naturally perform planning, implementation, and first-pass verification in one conversation. Requiring artificial sub-agent sessions would reproduce a Claude-specific implementation detail rather than preserve the underlying engineering principle.

Luna instead uses:

- conversation state for orchestration;
- sandbox for implementation;
- GitHub for durable state;
- CI/commands for deterministic gates;
- a separate review pass, human, model, or evidence comparison for 5-B.

## What is deliberately not copied

The source repository also contains presentation and logging infrastructure. These are useful demonstration artifacts, but they are not required for the workflow itself. The Luna repository does not copy the HTML slides, raw session logs, or Claude JSONL-to-HTML converter into the normative workflow.
