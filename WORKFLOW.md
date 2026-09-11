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
  → GATE
  → REVIEW
  → SHIP
  → LEARN
  → CONTEXT UPDATE
  → DONE
```

A failed gate returns to implementation. A failed independent review returns to specification.

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

Every requirement should have an observable acceptance condition where practical. The specification becomes the grading rubric for 5-B.

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

ChatGPT may act as both orchestrator and implementer. What must remain separate is **execution and final judgment**, not necessarily the conversational session.

## 5-A. GATE

Run deterministic checks appropriate to the project, for example:

- build;
- unit/integration tests;
- type checks;
- lint/format checks;
- packaging checks;
- schema validation;
- requirement/spec coverage;
- secret/credential scans;
- Git diff sanity;
- generated artifact checks.

A failed gate returns to implementation. Retries are bounded. Default `K=3`; complex code may use `K=6` when justified.

Do not claim a check passed unless it actually ran and its result is available.

Passing 5-A means the deterministic checks passed. It does **not** prove that the result is correct.

## 5-B. INDEPENDENT REVIEW

Perform an independent judgment against the specification.

Possible judges include:

- a separate model/review pass;
- a human reviewer;
- a before/after comparison;
- a baseline comparison;
- source/reference cross-checking;
- a domain owner.

The implementation process must not be the sole source of truth for its own correctness.

If 5-B fails, return to **2. Specification**. Do not simply rewrite the implementation until a reviewer happens to accept it. A review failure may indicate that the destination itself was underspecified.

## 6. SHIP & LEARN

Produce the requested durable result.

Record:

- what changed;
- what was verified;
- what was not verified;
- known limitations;
- decisions made during implementation;
- reusable lessons or new project rules.

Feed durable lessons back into the project's context so the next loop starts from a better baseline.

## Risk-based verification

| Risk | Typical examples | Minimum 5-B |
|---|---|---|
| R0 Low | wording, formatting, simple summaries | source/before-after check |
| R1 Medium | refactors, planning, ordinary research | independent review + evidence check |
| R2 High | schema changes, important metrics, broad behavior changes | domain/technical cross-review |
| R3 Very high | legal, privacy, financial, destructive automation | approved sources + explicit human approval + audit trail |

Increase verification strength when the blast radius, irreversibility, or uncertainty increases.

## Automation levels

- **Human:** human owns most workflow stages.
- **Human-in-the-loop:** human owns ambiguity, specification, and/or final judgment.
- **Human-on-the-loop:** human mainly maintains context and acceptance criteria while deterministic and independent checks are automated.

Automation is earned by making the acceptance criteria objectively testable. Do not skip validation merely because implementation is automated.

## Seven principles

1. Orchestration controls flow; execution performs work.
2. Execution inputs must contain enough context to avoid avoidable ambiguity.
3. Independent work may be parallelized.
4. Every retry loop has a bound.
5. Durable context is a product, not disposable prompt text.
6. Evaluation creates the improvement flywheel.
7. Decompose work to fit the available context and verification capacity.

## Completion rule

A task is complete only when the available evidence supports the stated completion definition.

Never substitute confidence, effort, or elapsed time for evidence.
