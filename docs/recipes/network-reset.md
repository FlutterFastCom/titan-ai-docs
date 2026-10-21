---
id: recipes/network-reset
audience: author
intents: [reset network, new ip, reset network settings, airplane, fresh connection]
tools: [list_tools, create_strategy, save_definition, create_release]
sources:
  - conductor:missions/2026-10-07-strategy-sharing-every-user-creates-prompt-parts/RELIABILITY-QUEUE.md
  - be:src/domain/tools/registry.rs
  - be:src/application/tools/dispatchable_catalog.rs
verified_be: 92a0a9c1
verified_conductor: 732df11a
---
# Recipe — reset the phone's network settings   (PROVEN)

One AI step with the `reset_network_settings` tool: it navigates, confirms, and waits for the phone to be usable again
(about 2 minutes offline in the recorded run). Mark the step `irreversible`.

Needs: a supervised iPhone set to English, awake and unlocked at the start (it does not unlock a locked phone).
Success means the reset was confirmed and the phone came back — it does not independently prove the network data was
erased. Repeating resets many times in a row is not proven.

This is NOT the same as toggling airplane mode, restarting the phone, or changing an eSIM. If the user says "new IP"
or "restart the eSIM", ask which of those they actually mean.

## Ask
"Which phone? Reset once before the rest, or between every round?"
