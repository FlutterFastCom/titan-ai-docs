---
id: devices/index
audience: both
intents: [phone problem, device not working, phone offline, connection problem, device not ready]
tools: [list_devices, device_detail, get_device_summary, get_host_deps, get_device_screen]
sources:
  - be:src/domain/device_status_policy.rs
  - be:src/domain/agents.rs
  - fe:src/shared/i18n/locales/en/devices.json
verified_be: dda945b4
verified_fe: 80c74183
verified_agent: 1814365
---
# Phone problems — find the right page

Your tools cannot restart, repair, pair, unlock or install anything on the phone or the PC. Every connection fix is
either "wait for Titan's automatic recovery" or "ask the user to do one thing", then you look again. Once a phone is
connected, you can see its screen and do simple things on it (tap, go Home, close an app) — that is for a phone or run
stuck on the wrong screen, not for connection problems (`devices/control-the-phone.md`).

## First, always
1. `get_device_summary`, then `list_devices` (small pages) and `device_detail` for the phone in question. Device tools
   take the phone's UDID (`deviceId`), not the numeric `rowId`.
2. Check freshness before anything else: an old snapshot is not the phone's current state (`devices/device-states.md`).
3. `get_host_deps` — is the PC side healthy?

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
| before suggesting anything unusual | `devices/never-do.md` |

## How to talk about it
Say what the user should do with their hands, one thing at a time: "Unlock the iPhone and keep it on the desk —
tell me when you have." Never say tunnel, WDA, usbmux, DDI, UDID; say "the phone connection" or "the automation link".
After each user action, read the state again before the next step.
