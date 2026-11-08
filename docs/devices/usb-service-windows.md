---
id: devices/usb-service-windows
audience: both
intents: [windows usb, apple mobile device service, usbmux, itunes driver]
tools: [device_detail, get_host_deps]
---
# Windows: the Apple USB service

Windows talks to iPhones through Apple's "Apple Mobile Device Service". A fresh Windows does not have it and the
Titan installer does not include it: it is a required one-time setup step on every Windows PC. Missing or stuck → no
phone appears, ever.

0. **First-time setup / service missing (Host Deps `amds` row not ok):** "Open PowerShell **as Administrator** and run
   `winget install --id Apple.AppleMobileDeviceSupport --accept-package-agreements --accept-source-agreements`, then
   `sc.exe start "Apple Mobile Device Service"`. Check: `(Get-Service -Name 'Apple Mobile Device Service').Status`
   must print `Running`." If `winget` is not recognized, the user installs **App Installer** from the Microsoft Store
   first. Never suggest the Microsoft Store **Apple Devices** app (`9NP83LWLPZ9K`): it does not register this service,
   so the `amds` row can never turn ok with it.
1. **User:** "Open PowerShell **as Administrator** and run:
   `Restart-Service -Name 'Apple Mobile Device Service' -Force`
   Wait a few seconds, unplug and replug the unlocked phone."
2. Installed but still not `Running`, or the phone still does not appear: "Restart the computer, then replug the
   unlocked phone."
3. **You:** look again. Still not detected → `collect-logs.md`.

Notes: use `sc.exe`, never bare `sc` in PowerShell (it means something else there). Do not ask the user to install
iTunes, Python or any other driver package instead.

On Mac and Linux there is no confirmed user command for this layer — keep the phone plugged in and unlocked, then
`collect-logs.md`.
