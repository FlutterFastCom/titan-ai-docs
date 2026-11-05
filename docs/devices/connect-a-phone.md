---
id: devices/connect-a-phone
audience: both
intents: [connect a new phone, add device, set up phone, install agent, pair]
tools: [list_devices, device_detail, get_host_deps, get_device_summary]
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
1. "In Titan, open **PC Agent** in the left menu and click **Get setup code**."
2. "Copy the setup code into the agent's pairing screen and click **Confirm** once." The code is shown only once.
3. "Wait — don't make a new code while it is connecting."
The web app moves from "Not connected" to "Waiting for the agent to connect…" to "Connected".
- Agent says **"Paired — relaunch to finish"**: "Click **Quit agent**, then open the app again. Don't make a new code."
- Web app says **"Disconnected"**: the pairing worked; the agent is not running → step 1's "open the agent" again.
- Only one computer can be linked to an account. A new setup code on a paired account (**Connect a different
  computer**) ends the old computer's pairing at once — never do it "to refresh".

## 3. Plug in the phone and add it (user, any OS)
1. "Connect the iPhone directly with a USB data cable (not a hub, not a charge-only cable). Unlock it."
2. Windows and Linux: "In Titan, open **Devices**, click **Connect a phone**, then **Connect** next to your iPhone
   under **Available on this computer**." If the phone is already in the Devices list (on a Mac it may be added by
   itself), skip this. Connecting a phone does not start a run.
3. "If it asks **Trust This Computer**, tap **Trust** and enter the passcode." In Titan, click **Retry** after that if
   it asks.
4. "Open **Settings → Privacy & Security → Developer Mode**, turn it on and confirm." It cannot be turned on remotely.
Finish the phone's first-time setup screens before this if needed.

## 4. Watch it become ready (you)
`list_devices` / `device_detail` until the phone shows fresh and ready. On the way it passes USB → trust → tunnel →
automation bridge → ready. If it stops on one of them for more than a few minutes, open the matching page from
`devices/index.md`. "Missing WDA assignment" needs an administrator: "Ask your Titan administrator to assign the
automation build for your phone."

The **Add device** page in the web app is not a setup wizard (it says "Device setup unavailable") — never send the
user there to connect a phone.
