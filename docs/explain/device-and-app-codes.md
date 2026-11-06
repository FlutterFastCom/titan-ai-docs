---
id: explain/device-and-app-codes
audience: both
intents: [phone not ready, app not installed, app launch failed, device command failed, not confirmed, may have executed, wda assignment, trust required, vision unavailable, snapshot missing, catalog unavailable, why did the phone action fail]
tools: [list_devices, device_detail, get_device_screen, device_action, list_device_apps, set_device_app_choice, list_agent_diagnostics, read_run_transcript, wait_seconds]
error_codes: [device_trust_required, device_command_timeout, device_command_uncertain, device_session_unavailable, device_wrong_state, device_command_rejected, device_vision_unavailable, device_vision_unmeasured, device_screen_unavailable]
---
# Phone and app codes — what each means and who can fix it

Where you see them: the `guidance` list on a phone (`device_detail`), a refusal from a phone tool, and the replies
inside a run's steps (`read_run_transcript`). Never show a code to the user: say the plain meaning. Rule for every row
marked "may have happened": look at `get_device_screen` before doing anything else, and never repeat the action blind.

## On a phone's `guidance` list (what to tell the user about a phone that is not ready)
| code | meaning | you can | ask the user | only Titan |
|---|---|---|---|---|
| `snapshot_missing` | Titan has not received any status for this phone yet | `wait_seconds`, then `device_detail` again | if it stays missing for minutes: "Is the Titan PC Agent open on your computer?" (`devices/connect-a-phone.md`) | — |
| `automatic_recovery` | the PC Agent is measuring or retrying a check by itself | wait and look again; never act on the phone meanwhile | nothing | if it never ends: `devices/collect-logs.md` |
| `trust_required` | the phone has not trusted this computer | nothing | "Unlock the iPhone and tap Trust, then enter the passcode." | — |
| `wda_assignment_missing` | no automation build is assigned to this phone | nothing | "Ask your Titan administrator to assign the automation build for your phone." | an administrator assigns it |
| `reason_unknown` | the phone is not ready and its report names no known cause | read every part in `device_detail` (`devices/device-states.md`), then `list_agent_diagnostics` (`devices/agent-diagnostics.md`) | one small hands-on check if a part points to one | tell the user plainly that you cannot find the cause and that Titan support has to look; offer `devices/collect-logs.md` |
The other guidance codes (`snapshot_stale`, `agent_disconnected`, `usb_detached`, `screen_asleep`) are in
`devices/device-states.md`.

`device_trust_required` is the same problem refused while connecting a phone: the PC Agent has already shown the Trust
question on the phone. After the user taps Trust, connect again once.

## In a run's step replies
| code | meaning | you can | ask the user | only Titan |
|---|---|---|---|---|
| `device_not_found` | no phone with that identifier on this account (removed, never added, or the numeric row id was used instead of the UDID) | `list_devices` and copy the UDID (`deviceId`) | if the phone is missing from the list: "Is it plugged in and added in Titan?" (`devices/phone-not-detected.md`) | — |
| `device_not_ready` | the phone could not take the action now. Read the reply's `delivery`: `not_sent` means nothing happened; anything else means it may have happened, so `get_device_screen` first | `device_detail`; follow `devices/index.md` | the one hands-on step that page names | — |
| `device_timeout` | no confirmation arrived in time; the action may have happened | `get_device_screen` to see the real state | nothing yet | repeated timeouts on a ready phone: collect logs |
| `device_rejected` | the phone side did not confirm the action; it may have happened | `get_device_screen`, then `list_agent_diagnostics` | nothing yet | the cause is not in the code; read the failure cause below |
| `device_unavailable` | the outcome is unresolved (the link dropped mid-action); it may have happened | `device_detail`, then `get_device_screen` | "Keep the phone plugged in and unlocked." | — |
| `vision_unavailable` | Titan could not read the screen for this step; the action's effect is unknown | `get_device_screen` once; if it also refuses, treat the phone as not ready | "Wake and unlock the phone." | repeated on a ready phone: collect logs |
| `vision_unmeasured` | Titan has no usable screen reading yet; effect unknown | wait a few seconds, `get_device_screen` | nothing | same as above |
| `app_catalog_unavailable` | Titan's list of known apps could not be read right now | wait, retry the same read once | nothing | still failing a few minutes later: Titan support |
| `install_app_store_id_missing` | the app the step wanted to install has no App Store entry on file at Titan | nothing: you cannot add it | ask the user to install the app on the phone by hand, then `list_device_apps` | Titan must add the entry |
| `tool_contract_unavailable` | one phone action of the strategy could not be prepared; it also happens when the strategy's saved tool contract or release lacks a tool the step uses | retry the run once; if the strategy is the user's own, re-save and re-release it with the tool included (`build/7-improve-it.md`) | if it is a shared strategy: "the person who made it has to fix it" | still failing after that: Titan support |

## In REST/MCP phone-tool refusals (`device_action`, `get_device_screen`, `list_device_apps`)
| code | meaning | you can | only Titan |
|---|---|---|---|
| `device_command_timeout` | no confirmation arrived in time; the action may have happened | `get_device_screen` first, never resend blind | repeated on a ready phone: collect logs |
| `device_command_uncertain` | the result is unknown; the action may have happened | `get_device_screen` first | same |
| `device_session_unavailable` | the phone link dropped; the action may have happened | `device_detail`, then `get_device_screen`; "Keep the phone plugged in and unlocked." | — |
| `device_wrong_state` | the phone is not ready for this; nothing was sent | `device_detail`, `devices/index.md` | — |
| `device_command_rejected` | the PC Agent refused the action | read `diagnostic.cause` and `stage` (below); `get_device_screen` | cause not listed: `list_agent_diagnostics` |
| `device_vision_unavailable`, `device_vision_unmeasured`, `device_screen_unavailable` | no usable screen reading; read-only refusals, nothing was changed | wake and unlock the phone, retry only the read later | repeated on a ready phone: collect logs |

## Failure cause on a failed phone action (when a refusal or a run carries it)
A refused phone action, and a failed run (`lastCommandFailure`), can carry a closed diagnosis: a **cause** and a
**stage**. It is evidence about the most recent failed command, not proof that it ended the run.
| cause | meaning | do |
|---|---|---|
| `app_not_installed` | a fresh app list proves the app is not on the phone, or (list unreadable, strategy steps) every approved version was tried and refused as not installed | `devices/installed-apps.md` |
| `app_choice_required` | several approved versions of the app are installed and none is chosen | `devices/installed-apps.md` |
| `wda_rejected` / `wda_unreachable` | the phone's automation link refused / did not answer | `devices/automation-bridge.md` |
| `unknown_operation` | the PC Agent is too old to know this action | `devices/agent-update.md` |
| `device_not_ready` | the phone was not ready | `devices/index.md` |
| `inventory_unavailable` | the app list could not be read (this is NOT proof the app is missing) | wait, `device_detail`, try once more |

Stage says how far it got: `admission` and `inventory` = refused before the phone was asked to do anything (safe to
fix and try again); `execution` and `verification` = the phone may have done it, so look at the screen first.
Only the agent-reported diagnostics (`devices/agent-diagnostics.md`) also carry a **remedy** hint.
