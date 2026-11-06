---
id: reference/tools-agent-pairing
generated: true
audience: both
---

# Tools — agent-pairing (1)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.

> A capitalised type name in an input (for example `QueueArgRequest`) is spelled out in request-shapes.md.


## list_agent_diagnostics
- does: Read safe, bounded diagnostics the owned agent reported about WARN/ERROR conditions: code, cause, stage, repeat count and drop counts. Optional query page, size and deviceId filter one phone; drop counters stay agent-wide. Labels are agent-reported observations, not proof a command ran or a phone is ready. A foreign or missing agent reads as agent_not_found.
- input: path { agent_id: string } · query { page?: integer; size?: integer; deviceId?: string }
- returns: AgentDiagnosticsPageResponse
- flags: read-only
