---
id: reference/tools-runs
generated: true
audience: both
---

# Tools — runs (5)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.

> A capitalised type name in an input (for example `QueueArgRequest`) is spelled out in request-shapes.md.


## create_run
- does: Create a run without starting it: create does not play, so call play_run explicitly. The response returns its runId. Caller rights and exact pins apply. A stable supplied operationId identifies the same request: a retry with it returns the original runId for 24 hours (receipt scoped to your owner and environment), a changed request under that operationId conflicts, and your rights and grant are rechecked on replay. Without operationId, an identical request recovers the original run for 120 seconds or while that run is still created. Receipts are bounded: never assume indefinite, cross-owner or unconditional replay, and do not use a fresh operationId to recover an uncertain request; read your runs first.
- input: body CreateRunSchema (exactly one of: OwnedCreateRunSchema | ReleaseCreateRunSchema — see request-shapes)
- returns: CreateRunResponse
- flags: changes data

## advance_run
- does: Advance a paused run you own; an empty response means the advance was accepted. Caller rights and exact pins apply.
- input: path { run_id: string }
- returns: (empty)
- flags: changes data

## cancel_run
- does: Destructive: cancel a run you own, ending it; the response returns its runId, status and outcome. Caller rights and exact pins apply.
- input: path { run_id: string }
- returns: RunTerminationResponse
- flags: changes data · destructive: confirm with the user first

## copy_run
- does: Copy a run you own into a new run without starting it; the response returns the new runId. Caller rights and exact pins apply. A lost response has no keyed receipt: read your runs before retrying.
- input: path { run_id: string }
- returns: CreateRunResponse
- flags: changes data

## play_run
- does: Play a run you own; an empty response means the start was accepted, not that it finished. Caller rights and exact pins apply.
- input: path { run_id: string }
- returns: (empty)
- flags: changes data
