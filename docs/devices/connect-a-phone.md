---
id: devices/connect-a-phone
audience: both
intents: [connect a new phone, add device, set up phone, install agent, pair]
tools: [list_devices, device_detail, get_host_deps, get_device_summary]
sources:
  - agent:scripts/packaging/installer/TitanAgent.iss
  - agent:docs/TRIAGE-DEVICE-NOT-DETECTED.md
  - fe:src/shared/i18n/locales/en/common.json
  - fe:src/shared/i18n/locales/en/devices.json
  - agent:ui/src/main.jsx
verified_be: 92a0a9c1
verified_fe: a93e3f30
verified_agent: 2ca0430
---
# Connect a new iPhone

You can only watch progress. The user does every step on their PC and phone. Give one step, wait, check, then the next.

## 1. Install the Titan PC Agent (user)
- **Windows:** "Run the Titan installer you were given and approve the administrator prompt. Then open **Titan PC
  Agent** from the Start menu." (Windows 10 or newer, 64-bit. The agent does not start by itself after install.)
  Windows also needs **Apple Mobile Device Support**; if Host Deps reports it missing: "Open PowerShell and run
  `winget install --id Apple.AppleMobileDeviceSupport --accept-package-agreements --accept-source-agreements`."
- **Mac:** "Run the Titan `.pkg` installer you were given, then open the Titan app from Applications." (Apple silicon,
  macOS 11 or newer.)
- **Linux:** "Ask whoever gave you Titan for the Linux install and start method." There is no confirmed self-service
  Linux installer — do not invent one.

## 2. Pair the PC with the account (user)
1. "In Titan, open **PC Agent** in the left menu and click **Connect**."
2. "Copy the setup code into the agent's pairing screen and click **Confirm** once."
3. "Wait — don't make a new code while it is connecting."
The web app moves from "Connect your machine" to "Waiting for the agent to connect…" to "PC Agent connected".
- Agent says **"Paired — relaunch to finish"**: "Click **Quit agent**, then open the app again. Don't make a new code."
- Web app says **"Agent registered — currently offline"**: the pairing worked; the agent is not running → step 1's
  "open the agent" again.

## 3. Plug in the phone (user, any OS)
1. "Connect the iPhone directly with a USB data cable (not a hub, not a charge-only cable). Unlock it."
2. "If it asks **Trust This Computer**, tap **Trust** and enter the passcode."
3. "Open **Settings → Privacy & Security → Developer Mode**, turn it on and confirm." It cannot be turned on remotely.
Finish the phone's first-time setup screens before this if needed.

## 4. Watch it become ready (you)
`list_devices` / `device_detail` until the phone shows fresh and ready. On the way it passes USB → trust → tunnel →
automation bridge → ready. If it stops on one of them for more than a few minutes, open the matching page from
`devices/index.md`. "Missing WDA assignment" needs an administrator: "Ask your Titan administrator to assign the
automation build for your phone."

The **Add device** page in the web app is not a setup wizard (it says "Device setup unavailable") — never send the
user there to connect a phone.
