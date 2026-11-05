---
id: devices/usb-service-windows
audience: both
intents: [windows usb, apple mobile device service, usbmux, itunes driver]
tools: [device_detail, get_host_deps]
---
# Windows: the Apple USB service

Windows talks to iPhones through Apple's "Apple Mobile Device Service". When it is stuck, no phone appears.

1. **User:** "Open PowerShell **as Administrator** and run:
   `Restart-Service -Name 'Apple Mobile Device Service' -Force`
   Wait a few seconds, unplug and replug the unlocked phone."
2. Service missing: "Run `winget install --id Apple.AppleMobileDeviceSupport --accept-package-agreements
   --accept-source-agreements`, then restart the computer."
3. **You:** look again. Still not detected → `collect-logs.md`.

Notes: use `sc.exe`, never bare `sc` in PowerShell (it means something else there). Do not ask the user to install
iTunes, Python or any other driver package instead.

On Mac and Linux there is no confirmed user command for this layer — keep the phone plugged in and unlocked, then
`collect-logs.md`.
