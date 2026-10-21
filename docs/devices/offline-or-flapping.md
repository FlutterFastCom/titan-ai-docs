---
id: devices/offline-or-flapping
audience: both
intents: [phone offline, keeps disconnecting, flapping, agent offline, stale]
tools: [get_host_deps, list_devices, device_detail]
sources:
  - be:src/application/agents/pairing.rs
  - agent:docs/AI-CLIENT-TRIAGE.md
  - agent:src/app/device_status_discovery_attempt.rs
  - agent:src/infra/instance_lock.rs
verified_be: 92a0a9c1
verified_fe: a93e3f30
verified_agent: 2ca0430
---
# Phones show offline, stale, or keep dropping

1. **You:** compare `get_host_deps` and the phones' last report times. All phones stale/disconnected at once → the PC
   Agent is the problem, not the phones.
2. **User — start the agent (only one copy):**
   - Windows: "Open **Titan PC Agent** from the Start menu. If it's already open, leave it — don't open a second one."
   - Mac: "Open the Titan app from Applications. Don't pair it again."
   - Linux: "Start the agent the way you were shown. Don't start a second copy."
3. **User:** "Check the computer is online and that no firewall or VPN on the computer blocks Titan. Keep the phone
   plugged in, unlocked and awake."
4. **You:** look again. If one phone keeps flipping between online and offline while others are fine, check its USB
   (`phone-not-detected.md` steps 1–2).
5. Still dropping after these: `collect-logs.md`.

Never suggest making a new pairing code, and never two agents on one computer — two copies on one pairing knock each
other offline. A leftover lock file is not a problem to delete.
