# Luna v2 Design Rationale

## 1. Workflow and runtime are separate products

The most important architectural decision is separation of concerns.

- `luna-agentic-workflow` defines **how reliable agentic work should proceed**.
- `eo4992-boop/gpt` defines **how Luna Chat Coder maintains continuity, repository state, and fallback behavior in ChatGPT**.
- An application repository defines **what the product actually is**.

This prevents a workflow experiment from destabilizing the existing Luna runtime.

## 2. New programs do not need new workflow repositories

The workflow repository is reusable infrastructure. Creating a new application should normally follow:

```text
new application repo
      ↓
read/apply Luna workflow
      ↓
create project-specific context/rules
      ↓
execute tasks
      ↓
verify
      ↓
learn back into that project's context
```

Only create another workflow repository if there is a genuinely different workflow product, organizational boundary, or experimental fork.

## 3. GitHub is the durable boundary

Chat history is useful for reasoning but should not be the only durable record of project state.

GitHub provides:

- source history;
- branches;
- pull requests;
- review discussion;
- CI evidence;
- artifacts;
- reproducible handoff.

## 4. Verification is the central reliability mechanism

The source playbook correctly moves attention from execution toward verification. Luna makes that explicit with 5-A and 5-B.

5-A answers: **Did the deterministic checks pass?**

5-B answers: **Does this actually satisfy the intended result?**

Those are different questions and should not be collapsed.

## 5. Review failure returns to specification

A reviewer rejecting the result can mean:

- implementation is wrong;
- acceptance criteria are incomplete;
- assumptions were wrong;
- the requested outcome was misunderstood.

Therefore a meaningful 5-B failure returns to specification rather than creating an unlimited implementation loop.

## 6. Risk controls the amount of ceremony

Luna should not make a one-line documentation change feel like a regulated deployment.

Low-risk work can use lightweight evidence. High-risk or irreversible work requires stronger independent review and human approval.

## 7. Evidence over claims

The workflow deliberately distinguishes:

- "I believe this works";
- "the test passed";
- "an independent review found no specification violation."

Only the latter two are verification evidence, and each supports a different claim.
