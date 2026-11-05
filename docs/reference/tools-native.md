---
id: reference/tools-native
generated: true
audience: both
---

# Tools — native (1)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.

> A capitalised type name in an input (for example `QueueArgRequest`) is spelled out in request-shapes.md.


## wait_seconds
- does: Pause 1–30 seconds between calls: after mcp_rate_limited until retryAfterSeconds has passed, or between live-state polls. Reserves no quota and proves no state change.
- input: seconds: integer 1..30
- returns: elapsed time
- flags: read-only
