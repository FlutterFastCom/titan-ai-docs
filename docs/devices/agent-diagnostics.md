---
id: devices/agent-diagnostics
audience: both
intents: [why is the phone not working, agent diagnostics, what did the agent report, phone offline or unverified, developer verification, find the cause, phone problem cause]
tools: [list_agent_diagnostics, device_detail, list_devices, get_device_screen, wait_seconds]
error_codes: [agent_not_found, invalid_page, invalid_page_size]
---
# Read what the PC Agent itself reported (`list_agent_diagnostics`)

Use it when a phone or run problem needs a cause and the phone's own status (`device_detail`) is not enough. It shows
the warnings and errors the Titan PC Agent reported, as short labelled entries. It is evidence from the agent, not proof
that a command ran or that a phone is ready.

## How to call it
1. `device_detail` for the phone: copy `status.agentId`. The tool takes that agent id in the path.
2. `list_agent_diagnostics({path:{agent_id}, query:{deviceId, page, size}})`. `deviceId` (the phone's UDID) is optional and
   narrows the entries to one phone; `page` starts at 1; `size` is 1 to 100 (default 25). Newest first.
3. A missing or someone else's agent id answers `agent_not_found`: re-read the id from `device_detail`.

## What comes back
- `items`: each has an `entry` with `code`, `level` (warn or error), `cause`, `stage`, optional `remedy`, optional
  `deviceId`, how many times it repeated (`occurrenceCount`), and when the agent first and last saw it. Around it:
  `firstReceivedAt` and `lastAdvancedAt` are Titan's own clock; the first/last-observed times are the agent's clock. Do not
  compare the two clocks to each other.
- `drops`: how many entries the agent had to drop (it keeps a bounded queue), per agent start. These counts are for the
  whole agent even when you filter by phone.
- Repeat and drop counts are totals since that agent start or observation window, not lifetime totals. Entries older
  than about a week disappear.

## Reading the entries
| code (what the agent saw) | plain meaning | say / do |
|---|---|---|
| `wda_assignment_missing` | no automation build assigned to the phone | an administrator must assign it (`explain/device-and-app-codes.md`) |
| `wda_assignment_rate_limited` | the agent asked for the assignment too often and is backing off | wait; look again in a few minutes |
| `wda_install_failed`, `wda_launch_failed`, `wda_proxy_timeout`, `wda_proxy_rejected` | the automation link on the phone could not be installed, started or reached | `devices/automation-bridge.md` |
| `developer_service_unavailable` | the phone's developer service did not answer | keep the phone unlocked and on; `devices/index.md` |
| `tunnel_start_failed`, `tunnel_proof_failed` | the phone connection (tunnel) did not start or was not proven | `devices/tunnel-down.md` |
| `fixed_flow_rejected` | one of Titan's built-in phone routines (network reset, VPN connect, app install, photo share) was refused | read `cause`; nothing to repeat blindly |
| `app_not_installed` | a required app is not on the phone | `devices/installed-apps.md` |
| `phone_offline_or_unverified` | the phone could not be verified, usually no internet on the phone or Developer verification not confirmed | the entry's remedy hint points the same way. Ask the user to check that the iPhone has internet (Wi-Fi or mobile data) and that its developer verification is confirmed, then unlock it and wait |
| `agent_internal_failure` | a fault inside the PC Agent | `devices/collect-logs.md` |
`cause` and `stage` narrow it further (for example stage `developer_verification` or `wda_install`). They are the
agent's own labels: report them in plain words, never as a certain root cause.

## When it does not help
Empty list, or only entries that do not match the symptom: say plainly that Titan has no recorded cause, then
`devices/collect-logs.md` and Titan support. Do not guess a cause and do not repeat actions to provoke one.
