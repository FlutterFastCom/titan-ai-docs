---
id: devices/collect-logs
audience: both
intents: [send logs, collect logs, diagnostics, support, escalate]
tools: [get_host_deps]
sources:
  - agent:src/infra/logging.rs
  - agent:src/infra/credential_store.rs
  - agent:docs/HANDOFF-COLLECT-AGENT-LOGS.md
verified_be: 92a0a9c1
verified_fe: a93e3f30
verified_agent: 2ca0430
---
# Collect the PC Agent logs (when the steps did not help)

Collect BEFORE restarting anything — only the current and two previous agent runs are kept.

| system | folder | say |
|---|---|---|
| Windows | `%APPDATA%\titan-agent\logs\runs` | "Press **Win+R**, paste this path, press Enter, and copy the `.log` files." |
| Mac | `~/Library/Application Support/titan-agent/logs/runs` | "In Finder choose **Go → Go to Folder**, paste this path, and copy the `.log` files." |
| Linux | `~/.local/share/titan-agent/logs/runs` | "Open this folder (show hidden files) and copy the `.log` files." |

Ask the user to send those files to their Titan support contact, with: their system (Windows/Mac/Linux), the agent
version, when it happened (with time zone), what they saw, and what was already tried.

Never ask for the whole Titan folder — it contains the agent's sign-in file. Never ask for passwords or passcodes.
Don't ask them to paste logs into this chat if they contain account names.
