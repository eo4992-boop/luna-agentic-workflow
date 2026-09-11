# Claude Code Specific Elements Removed

The source playbook is valuable because of its workflow logic, not because of its Claude Code session machinery.

## Removed or generalized

| Source-specific element | Luna v2 treatment |
|---|---|
| Claude Code as the assumed agent | ChatGPT is the reference adapter; core remains host-neutral |
| Dedicated Claude orchestrator session | ChatGPT conversation can own orchestration |
| Mandatory sub-agent session per chunk | Decomposition remains; execution mechanism is environment-dependent |
| `~/.claude/...` paths | Replaced by repository/sandbox/GitHub references |
| Claude session JSONL logs | Not part of the workflow contract |
| `tools/log2html.py` | Not copied; logging format is not a workflow invariant |
| Team-mate/sub-agent session log viewer | Replaced conceptually by Git history, PRs, CI, review records, and evidence logs |
| Claude-specific demo prompts | Replaced by reusable Luna task/review templates |
| Local CLI assumptions | Replaced by ChatGPT + GitHub adapter |
| "Orchestrator never executes" as a hard runtime requirement | Narrowed to separation of orchestration from final judgment |

## Why the orchestrator rule changed

The source template says the orchestrator must manage flow and not directly execute work. That is a useful separation when a system has multiple sub-agents, but it is not necessary in ChatGPT's single conversational environment.

Luna preserves the important boundary:

> The implementation process must not be the sole judge of correctness.

Thus ChatGPT may plan and implement, but 5-B must still be independently grounded.

## Why sub-agents are optional

Sub-agents are an execution optimization, not the workflow itself. They can improve context isolation and parallelism when the host supports them. ChatGPT + GitHub does not need to imitate another agent runtime to benefit from decomposition, bounded tasks, and independent review.

## What remains portable

The following survive unchanged in spirit:

- persistent context;
- explicit intent;
- checklist specification;
- bounded decomposition;
- deterministic gates;
- independent review;
- bounded retries;
- risk-based verification;
- durable learning.
