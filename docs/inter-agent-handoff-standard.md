# Inter-Agent Handoff Standard (sessions_send)

Use this standard for **all** agent-to-agent requests in OpenClaw teams.

Telegram bots cannot reliably read each other, so handoffs must happen via `sessions_send`.

## Rules

1. Every handoff must include the required fields below.
2. Receiver must ACK quickly in the shared topic.
3. Receiver must post a completion update in the shared topic.
4. If blocked or overdue, receiver escalates with a blocker report.
5. No free-form handoffs for production workflows.

## Required Handoff Format

Send this exact structure in `sessions_send`:

```text
HANDOFF
from: <agent-id>
to: <agent-id>
task_id: <short-unique-id>
priority: P0|P1|P2
summary: <one-line task summary>
context: <facts, links, paths, branch, constraints>
deliver_to: telegram:<group-id>:<topic-id>
deadline: <ISO timestamp or "asap">
done_when:
- <acceptance criterion 1>
- <acceptance criterion 2>
```

## ACK Standard (Receiver)

Post this in the target topic within 2 minutes:

```text
ACK <task_id> — accepted
ETA: <time estimate>
```

If rejecting:

```text
ACK <task_id> — rejected
Reason: <clear reason>
Need: <what is required to proceed>
```

## Completion Standard (Receiver)

Post this when done:

```text
DONE <task_id>
Result: <what was delivered>
Evidence:
- <link/path/screenshot>
- <link/path/screenshot>
Risks/Notes: <optional>
```

## Escalation Standard

If blocked >15 minutes or deadline risked:

```text
BLOCKED <task_id>
Blocker: <what is blocked>
Impact: <what cannot proceed>
Need decision from: <agent-id or human>
Options:
1) <option + tradeoff>
2) <option + tradeoff>
Recommendation: <best option>
```

## Example

```text
HANDOFF
from: coder
to: qa
task_id: build-142
priority: P1
summary: Validate checkout fix for coupon rounding bug
context: branch=fix/coupon-rounding, env=staging, PR=#142
deliver_to: telegram:-1001234567890:13
deadline: 2026-03-08T10:00:00+02:00
done_when:
- Repro no longer fails on staging
- Regression checks for totals pass
```
