# Luna Agentic Workflow v2

## Status

Normative workflow specification.

## Purpose

Provide a reusable procedure for turning an ambiguous request into a verified, durable result while keeping the durable project state in GitHub.

This document is methodology, not a requirement to use a particular model, CLI, IDE, or sub-agent runtime.

## State machine

```text
INIT
  → CONTEXT
  → INTENT
  → SPEC
  → PLAN
  → IMPLEMENT
  → 5-A INDEPENDENT TECHNICAL GATE
  → 5-B USER ACCEPTANCE TEST
  → SHIP
  → LEARN
  → CONTEXT UPDATE
  → DONE
```

A failed 5-A gate returns to implementation. A failed 5-B user acceptance test returns to the appropriate specification/implementation stage.

## 0. CONTEXT

Read the project's durable context before changing anything:

- repository instructions and contribution rules;
- architecture and relevant source files;
- current GitHub branch/commit/PR state;
- prior decisions and known constraints;
- verification commands and expected evidence;
- known assumptions and unresolved risks.

Separate **facts**, **decisions**, **assumptions**, and **unknowns**. Never invent a project rule that is not present in the repository or explicitly supplied by the user.

The context should become a durable project asset. Do not force the same information to be rediscovered on every task.

## 1. INTENT

Restate the requested outcome and constraints.

Identify assumptions. If an unresolved assumption can materially change the outcome, stop and ask the user before implementation.

The goal is not to ask questions for their own sake. Ask only questions whose answers can change the result, scope, risk, or acceptance criteria.

## 2. SPECIFICATION

Convert the intent into a testable checklist.

Every requirement should have an observable acceptance condition where practical. The specification becomes the grading rubric for 5-A and the acceptance checklist for 5-B.

If an item cannot be evaluated, clarify it or explicitly classify it as an assumption.

## 3. PLAN / DECOMPOSE

Split the work into bounded tasks.

For each task record:

- purpose;
- inputs;
- outputs;
- dependencies;
- affected files/components;
- success criteria;
- verification method.

Independent tasks may be processed independently when doing so improves speed without increasing risk.

Do not require a literal "one sub-agent session per chunk". The original playbook used that as an implementation technique; Luna treats decomposition as a planning principle and chooses the execution mechanism available in the current environment.

## 4. IMPLEMENT

Use the best available engineering environment.

For ChatGPT + GitHub:

1. materialize/read the exact GitHub source state before editing;
2. preserve unrelated changes;
3. make bounded, reviewable changes;
4. keep implementation aligned to the specification;
5. record important deviations instead of silently changing scope.

The **main Chat** may act as orchestrator and implementer. What must remain separate is the final technical judgment in 5-A and the real-world acceptance judgment in 5-B.

## 5-A. INDEPENDENT TECHNICAL GATE

5-A is performed by an AI reviewer that is **not the main Chat that performed the implementation**.

The reviewer may be:

- another ChatGPT conversation;
- another AI model;
- a dedicated review chat;
- another coding/review agent;
- another independent review system.

The exact tool does not matter. The essential requirement is **independence from the main implementation Chat**.

The 5-A reviewer should inspect the actual current project state and evidence rather than trusting the main Chat's claim that the work is complete.

Review deterministic and technical conditions appropriate to the project, for example:

- build;
- unit/integration tests;
- type checks;
- lint/format checks;
- packaging/build artifact validity;
- schema validation;
- requirement/spec coverage;
- secret/credential scans;
- Git diff sanity;
- generated artifact checks;
- obvious correctness and regression risks.

The reviewer should return a clear **PASS / FAIL** judgment with evidence and specific findings.

A failed 5-A gate returns to implementation. Retries are bounded. Default `K=3`; complex code may use `K=6` when justified.

The main Chat must not declare 5-A passed merely because it believes the implementation is correct. The independent reviewer must provide the evidence.

## 5-B. USER ACCEPTANCE TEST

5-B is performed by the **user in the real target environment** whenever a usable executable, application, website, package, or other real-world result can be delivered.

For a desktop application, the preferred 5-B artifact is the actual distributable build, such as an `.exe`, installer, or packaged application. The user runs it on their own PC and verifies real behavior.

The user checks the result against the specification and reports:

- whether the requested behavior actually works;
- usability problems;
- unexpected behavior;
- environment-specific failures;
- false positives/false negatives;
- performance problems;
- data safety issues;
- anything that differs from the intended result.

The user's real-world test is independent of the AI implementation/review process and is especially important for GUI, hardware, filesystem, performance, compatibility, and workflow behavior that cannot be fully established from source inspection.

If 5-B fails, return to the relevant specification and implementation stage. Do not simply patch symptoms without checking whether the requirement or acceptance criterion itself needs clarification.

If the user cannot practically test the result, explicitly record 5-B as **NOT VERIFIED** rather than treating it as passed.

### 5-A vs 5-B

| Stage | Judge | Primary question |
|---|---|---|
| **5-A** | Independent AI other than the main implementation Chat | "Is this technically sound and does the implementation satisfy the specification based on the available evidence?" |
| **5-B** | User in the real target environment | "Does the delivered result actually work for me as intended?" |

Neither stage may be silently skipped. If one is not applicable, record why.

## 6. SHIP & LEARN

Produce the requested durable result only after the applicable validation stages have passed.

Record:

- what changed;
- what was verified by 5-A;
- what was verified by 5-B;
- what was not verified;
- known limitations;
- decisions made during implementation;
- reusable lessons or new project rules.

Feed durable lessons back into the project's context so the next loop starts from a better baseline.

## Risk-based verification

| Risk | Typical examples | Minimum validation |
|---|---|---|
| R0 Low | wording, formatting, simple summaries | 5-A may be lightweight; 5-B when a real artifact exists |
| R1 Medium | refactors, planning, ordinary applications | independent 5-A + user 5-B when applicable |
| R2 High | schema changes, important metrics, broad behavior changes | stronger independent 5-A + real-world 5-B + additional technical/domain review when needed |
| R3 Very high | legal, privacy, financial, destructive automation | independent technical review + explicit human approval + real-world acceptance + audit trail |

Increase verification strength when the blast radius, irreversibility, or uncertainty increases.

## Automation levels

- **Human:** human owns most workflow stages.
- **Human-in-the-loop:** human owns ambiguity, specification, and/or final acceptance.
- **Human-on-the-loop:** human mainly maintains context and acceptance criteria while deterministic and independent checks are automated.

For Luna v2, 5-B intentionally remains human-owned for deliverables that require real-world acceptance. Automation may prepare the evidence, but it must not fabricate user acceptance.

## Seven principles

1. Orchestration controls flow; execution performs work.
2. Execution inputs must contain enough context to avoid avoidable ambiguity.
3. Independent work may be parallelized.
4. Every retry loop has a bound.
5. Durable context is a product, not disposable prompt text.
6. Evaluation creates the improvement flywheel.
7. Decompose work to fit the available context and verification capacity.

## Completion rule

A task is complete only when the available evidence supports the stated completion definition and the applicable 5-A and 5-B validation stages have passed.

Never substitute confidence, effort, or elapsed time for evidence. Never report user acceptance as passed unless the user has actually performed the applicable real-world test.
