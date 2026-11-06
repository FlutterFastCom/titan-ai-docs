---
id: devices/collect-logs
audience: both
intents: [send logs, collect logs, diagnostics, support, escalate]
tools: [get_host_deps]
---
# Collect the PC Agent logs (when the steps did not help)

Collect BEFORE restarting anything. Old agent run logs are deleted oldest-first once all of them together pass a size
budget, so a long or noisy history can push out the run you need; the current run is always kept.

| system | folder | say |
|---|---|---|
| Windows | `%APPDATA%\titan-agent\logs\runs` | "Press **Win+R**, paste this path, press Enter, and copy the `.log` files." |
| Mac | `~/Library/Application Support/titan-agent/logs/runs` | "In Finder choose **Go → Go to Folder**, paste this path, and copy the `.log` files." |
| Linux | `~/.local/share/titan-agent/logs/runs` | "Open this folder (show hidden files) and copy the `.log` files." |

**Windows: the newest log is locked while the agent runs.** The file of the run in progress cannot be opened or copied
until the Titan PC Agent stops. Ask for all the other `.log` files first. If the problem is in the current run, then:
"Quit the Titan PC Agent, copy the newest `.log` file, then start the agent again." Do this only after
the other files are copied — stopping the agent ends the current run.

**Windows, when the phone connection (tunnel) is the problem:** the background tunnel service keeps its own protected log
in a second folder, which any user can read: `C:\ProgramData\Titan\DeviceAgent\logs\tunneld`. Ask for those `.log` files
too ("Press **Win+R**, paste this path, press Enter, and copy the files"). It records which step failed and why, never
the phone's data, and it survives restarts.

Ask the user to send those files to their Titan support contact, with: their system (Windows/Mac/Linux), the agent
version, when it happened (with time zone), what they saw, and what was already tried.

Never ask for the whole Titan folder — it contains the agent's sign-in file. Never ask for passwords or passcodes.
Don't ask them to paste logs into this chat if they contain account names.
