---
id: devices/phone-not-detected
audience: both
intents: [phone not showing, phone not found, not detected, missing phone, usb]
tools: [get_host_deps, list_devices, device_detail]
---
# The phone is not detected

First make sure the PC Agent itself is connected with a fresh report (`get_host_deps`, `list_devices`). If it is not,
go to `offline-or-flapping.md` — a missing phone is often a missing agent.

On Windows and Linux a plugged-in phone is not added by itself: it appears in the phone list only after the user adds
it, and a phone removed with **Forget phone** has left the list. Ask first: "In Titan, open **Devices** and
click **Connect a phone**. Is your iPhone listed under **Available on this computer**? Then click **Connect** next to
it (unlock the phone and tap Trust if it asks, then **Retry**)." Only if it is not listed there, continue below.

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
