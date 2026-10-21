---
id: devices/never-do
audience: both
intents: [unsafe, should I erase, factory reset, re-pair, reinstall everything]
tools: [list_devices]
sources:
  - agent:docs/AI-CLIENT-TRIAGE.md
  - agent:src/infra/credential_store.rs
  - fe:src/shared/i18n/locales/en/devices.json
  - be:src/application/agents/pairing.rs
verified_be: 92a0a9c1
verified_fe: a93e3f30
verified_agent: 2ca0430
---
# Never suggest these as a connection fix

- **Two PC Agents on one computer or one pairing** — they knock each other offline.
- **A new pairing code "to refresh"** — it cuts the existing connection.
- **Deleting, editing or sending the agent's sign-in file** (`agent-session.json`) or the whole Titan folder.
- **Editing, replacing or pip-installing into Titan's installed files**, or turning off their checks.
- **Running your own tunnel / Python commands** next to Titan's, killing processes in bulk, or restarting in a loop.
- **Erasing or factory-resetting the phone**, turning off its passcode, or supervising it, as a fix. Erase wipes the
  phone; it is a separate, deliberate setup step, never troubleshooting.
- **Claiming you approved Trust or Developer Mode** — only the user can, on the phone.
- **Claiming it is fixed** from an old screenshot, a stale report, or "the command was accepted" — only a fresh ready
  report proves it.
