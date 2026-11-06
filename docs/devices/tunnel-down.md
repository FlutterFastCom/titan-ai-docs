---
id: devices/tunnel-down
audience: both
intents: [tunnel, tunnel degraded, restart tunnel, ios 17, connection to phone failed]
tools: [device_detail, get_host_deps]
---
# The phone connection (tunnel) is down — iOS 17 and newer

You see: USB attached and trusted, but tunnel not ready/failed. The web app may say "Tunnel degraded — needs restart",
"Start tunnel", "Restart tunnel" or "Fix in Host Deps".

1. **You:** `device_detail` and `get_host_deps`; USB and trust must be fine first.
2. **User:** "Keep the phone plugged in, unlocked and awake." Titan restarts the tunnel by itself: it checks every few
   seconds and tries at most twice in five minutes. Give it about 2 minutes, then look again.
   On Windows the PC Agent also checks the tunnel service when it starts: it leaves a healthy one alone and starts one
   that is verified stopped.
3. Still down:
   - **Windows:** if it never comes up right after an install or update, ask the user to run the Titan installer again
     (it recreates the protected log folder the tunnel service needs). Otherwise "Open **Host Deps** in the left menu and use **Start** or **Repair** next to the tunnel." Don't run
     any Python or tunnel command yourself in a terminal.
   - **Mac:** "Use the repair option for the tunnel in Host Deps. If macOS asks for your password, approve it. If the
     Titan background item is switched off, enable **Titan Device Agent** in **System Settings → General → Login
     Items**."
   - **Linux:** "If your setup includes the Titan tunnel service, restart it: `sudo systemctl restart
     titan-tunneld.service`." If that service doesn't exist on their machine, go to `collect-logs.md`.
4. **You:** look again; a started command is not a fixed tunnel — wait for tunnel ready.
5. Two failed rounds: `collect-logs.md` (on Windows it names the tunnel service's own log folder). `devices/agent-diagnostics.md`
   may name the failing step (`tunnel_start_failed`, `tunnel_proof_failed`). Don't keep restarting.
