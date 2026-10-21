---
id: reference/tools-host-deps
generated: true
audience: both
---

# Tools — host-deps (1)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## get_host_deps
- does: Read your paired agents' host dependency status. Inspect connection.state and snapshot.state/receivedAt/agentGeneratedAt/staleAfterSeconds before interpreting dependency state. No agent or stale status is not ready. This key cannot invoke host refresh/actions; returned command/fallback hints are information, not MCP operation permission.
- input: none
- returns: HostDepsResponse
- flags: read-only
