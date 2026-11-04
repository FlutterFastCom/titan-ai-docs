---
id: devices/device-states
audience: both
intents: [what does ready mean, device status, stale, disconnected, blocked, fresh]
tools: [get_device_summary, list_devices, device_detail]
sources:
  - be:src/domain/device_status_policy.rs
  - be:src/domain/device_status_judgment.rs
  - be:src/domain/agents.rs
  - be:src/domain/facet_probe_state.rs
  - be:src/domain/agent_status_issue_code.rs
verified_be: dda945b4
verified_fe: 80c74183
verified_agent: 1814365
---
# What a phone's status means (for you)

## Freshness — check it first
| value | meaning | what to do |
|---|---|---|
| fresh | the PC Agent reported within the last ~2 minutes | now read readiness — fresh is NOT ready |
| stale | the agent is connected but its report is old | don't trust the old values; see `offline-or-flapping.md` |
| disconnected | no live agent connection | the agent is closed or offline; it does NOT mean the phone was unplugged |
| missing | never reported | the agent has not seen it yet |
Summary counts: `ready` = fresh AND ready; `blocked` = every phone that is not ready (stale and disconnected included);
`unknown` currently never increments — zero unknown proves nothing.

## The phone's stages
`detached` (not plugged / not seen) → `attached` → `trust_pending` (user must tap Trust) → `tunnel_starting` →
`wda_starting` → `ready`. Also: `busy` (doing a run — don't interfere unless the run is stuck:
`control-the-phone.md`), `recovering` (Titan is fixing it — wait),
`quarantined` (recovery gave up — collect logs), `unknown` (no evidence yet).

## Parts of the connection (from `device_detail`)
| part | values | plain fix |
|---|---|---|
| USB | attached, detached, unknown | plug in / other cable → `phone-not-detected.md` |
| trust | trusted, pending, untrusted, unknown | unlock and tap Trust |
| tunnel | not_started, starting, ready, failed, paused, unknown | `tunnel-down.md` |
| automation bridge (WDA) | not_started, installing, launching, session_opening, ready, failed, not_signed, wrong_version, reinstalling, unknown | `automation-bridge.md` |
| screen | awake, asleep, locked, unknown | wake / unlock → `phone-locked.md` |
| developer disk (DDI) | mounted, not_mounted, mounting, unknown | usually resolves itself; Developer Mode must be on |
| supervision | supervised, unsupervised, unknown | needed only for some tools (e.g. network reset); never a reason to erase |

## Issue codes you may see
`device_trust_pending` → tap Trust · `tunnel_failed` → `tunnel-down.md` · `wda_not_ready` → `automation-bridge.md` ·
`host_dep_missing` → `host-deps.md` · `recovery_in_progress` → wait and look again · `quarantined` → `collect-logs.md` ·
`observability_lost` / `presence_source_down` → Titan lost sight of it; this does not mean the phone is gone ·
`auth_missing` / `auth_invalid` → the PC Agent's sign-in is the problem; do NOT tell the user to re-pair before checking
`agent-update.md`.

## Hints on each part (`presentation`, when the PC Agent sends one)
`ok` → fine · `not_checked`, `checking`, `recovering`, `problem_auto` → Titan is on it: wait and look again ·
`waiting` with `waitingFor` → it waits for that other part; fix that one first · `problem_user` with
`instruction: true` → this is the one thing the user must do (the web app shows the same instruction, e.g. "Unlock
the phone and tap Trust.") · no hint or `unknown` → use the values and reasons below. A hint is advice, not a readiness verdict.
`tunnel_failed` is shown only while the tunnel is really failed now; an old failure that the agent's current check
overrules is dropped.

Reasons on a part: `dependency_unready`/`missing_deps` → fix what it is blocked by first · `probe_timeout`/
`probe_failed` → check again once, then logs · `user_action_required` → follow that part's instruction ·
`install_failed`/`launch_failed` → logs.
