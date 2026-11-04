---
id: devices/agent-update
audience: both
intents: [agent update, update required, agent stopped, agent not running, new version]
tools: [list_device_agent_updates, get_host_deps, list_devices]
sources:
  - be:src/domain/agent_release.rs
  - fe:src/entities/devices/libs/constants/agent-release.constants.ts
  - fe:src/shared/i18n/locales/en/common.json
  - agent:ui/src/main.jsx
verified_be: dda945b4
verified_fe: 80c74183
verified_agent: 1814365
---
# The PC Agent is stopped or needs an update

1. **You:** `list_device_agent_updates`. `update_required` = below the minimum: it must be updated. `advisory` = a newer
   one exists; fine for now. `complete=false` = Titan could not decide; don't conclude anything.
2. Agent not running → start it (`offline-or-flapping.md` step 2). "Connection issue" on the **PC Agent** page only
   means Titan couldn't refresh the status — click **Check status**, don't change anything on the PC.
3. **Update (user):** "First let me collect the logs if something is wrong (`collect-logs.md`). Then use **Download
   agent** in Titan if you see it, or get the current installer from whoever gave you Titan. Run it, open the agent
   again, and tell me when it shows connected."
4. Keep the existing pairing — an update does not need a new code.
5. **You:** confirm a fresh report and the new version.
