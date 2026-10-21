---
id: devices/phone-not-detected
audience: both
intents: [phone not showing, phone not found, not detected, missing phone, usb]
tools: [get_host_deps, list_devices, device_detail]
sources:
  - agent:docs/TRIAGE-DEVICE-NOT-DETECTED.md
  - agent:src/app/usb_disconnect_reconciler.rs
  - agent:src/infra/usbmux_listen.rs
verified_be: 92a0a9c1
verified_fe: a93e3f30
verified_agent: 2ca0430
---
# The phone is not detected

First make sure the PC Agent itself is connected with a fresh report (`get_host_deps`, `list_devices`). If it is not,
go to `offline-or-flapping.md` — a missing phone is often a missing agent.

1. **User:** "Unlock the iPhone and plug it straight into the computer with a USB data cable. If it asks to Trust,
   tap Trust." → wait, then `device_detail`.
2. **User:** "Try a different cable and a different USB port on the computer." A cable that charges may still not
   carry data.
3. **Windows:** "Open **Services** (press Win, type Services), find **Apple Mobile Device Service** and click Start if
   it is stopped." If it is not there at all: install Apple Mobile Device Support (`connect-a-phone.md` step 1). Still
   nothing → `usb-service-windows.md`.
4. **Mac / Linux:** "Keep the phone plugged in and unlocked." If it still does not appear, go to `collect-logs.md` —
   there is no confirmed self-service repair command on these systems.
5. **You:** look again after every step; stop as soon as the phone appears and continue from its next missing stage.

Do not suggest restarting the tunnel for this problem — the tunnel only matters once the phone is detected. Do not
suggest "Reset Location & Privacy" on the phone as a routine step.
