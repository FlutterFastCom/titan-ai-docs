---
id: devices/index
audience: both
intents: [phone problem, device not working, phone offline, connection problem, device not ready]
tools: [list_devices, device_detail, get_device_summary, get_host_deps, get_device_screen, wait_seconds]
error_codes: [devices_unavailable]
---
# Phone problems — find the right page

Your tools cannot restart, repair, pair, unlock or install anything on the phone or the PC. Every connection fix is
either "wait for Titan's automatic recovery" or "ask the user to do one thing", then you look again. Once a phone is
connected, you can see its screen and do simple things on it (tap, go Home, close an app) — that is for a phone or run
stuck on the wrong screen, not for connection problems (`devices/control-the-phone.md`).

## First, always
1. `get_device_summary`, then `list_devices` (small pages) and `device_detail` for the phone in question. Device tools
   take the phone's UDID (`deviceId`), not the numeric `rowId`.
2. Check freshness before anything else: an old snapshot is not the phone's current state. A phone whose status says
   `lastKnown` is shown as not ready and its values are the last ones heard (`devices/device-states.md`). Read the
   phone's `guidance` list too: it names the one next step.
3. `get_host_deps` — is the PC side healthy?

## If a phone tool refuses
| code | meaning | you can | ask the user | only Titan |
|---|---|---|---|---|
| `devices_unavailable` | Titan could not complete the request because a part of Titan behind the phone tools, or one of its processing steps, failed — it can come back from reading phones (list, one phone, screen or screenshot), from renaming a phone, and from phone actions and commands. It is on Titan's side, not the phone or the PC, and may pass on its own | a READ (phones, screen): wait a little (`wait_seconds`) and read once more. Anything that CHANGES something (rename, phone action, command): never repeat it blindly — read the phone first. Send it again only when that read shows the change clearly did not happen; if you cannot tell, stop and tell the user it is unclear | nothing to do on the phone or PC. Reading: "Titan can't show your phones right now — I'll try again shortly." Changing: "Titan couldn't confirm that just now — let me check whether it happened." | still failing after a few minutes: only Titan support can fix it |

## Then pick the page
| what you see | page |
|---|---|
| no phones at all, or the user wants to add one | `devices/connect-a-phone.md` |
| the phone is not in the list, or USB shows detached/unknown | `devices/phone-not-detected.md` |
| the phone is connected but stuck on a popup or the wrong app, or a run is stuck on the phone | `devices/control-the-phone.md` |
| everything "disconnected"/"stale", or the phone keeps going offline and online | `devices/offline-or-flapping.md` |
| USB and trust are fine but the tunnel is not ready (iOS 17+) | `devices/tunnel-down.md` |
| Windows, phone never enumerates, Apple service problems | `devices/usb-service-windows.md` |
| the automation bridge (WDA) is not ready, failed, wrong version, not signed | `devices/automation-bridge.md` |
| screen locked or asleep | `devices/phone-locked.md` |
| the PC Agent is offline or needs an update | `devices/agent-update.md` |
| Host Deps shows something missing | `devices/host-deps.md` |
| still broken after the page's steps | `devices/collect-logs.md` |
| a phone action or run failed and you need the cause | `devices/agent-diagnostics.md`, `explain/device-and-app-codes.md` |
| an app is missing, or two versions of an app are installed | `devices/installed-apps.md` |
| before suggesting anything unusual | `devices/never-do.md` |

## How to talk about it
Say what the user should do with their hands, one thing at a time: "Unlock the iPhone and keep it on the desk —
tell me when you have." Never say tunnel, WDA, usbmux, DDI, UDID; say "the phone connection" or "the automation link".
After each user action, read the state again before the next step.
