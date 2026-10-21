---
id: now/what-to-do-now
audience: both
intents: [what should I do now, where do I start, what next, is everything ready, help me get started, status]
tools: [get_device_summary, list_devices, list_device_agent_updates, get_host_deps, list_user_strategies, list_user_presets, list_resources, list_user_runs, list_queues]
sources:
  - be:src/adapters/http/auth_api_keys_catalog.rs
  - be:src/domain/device_status_policy.rs
verified_be: 92a0a9c1
---
# "What should I do now?"

Never answer from memory. Read the state in this order, stop at the FIRST row that applies, and give the user that one
next step in plain words. Do the reads silently — the user only sees your conclusion.

## The reads (in this order)
1. `get_device_summary` — counts: total, ready, fresh, stale, disconnected, blocked, unknown.
2. `list_device_agent_updates` — any phone whose PC Agent needs an update.
3. `get_host_deps` — whether the user's PC has what the phone connection needs.
4. `list_queues` and `list_user_runs` (newest first, a few rows) — anything running, anything that just failed.
5. `list_user_strategies` — strategies they own, that were shared with them, or that are public.
6. `list_user_presets` — run settings they already saved.
7. `list_resources` — their saved accounts, descriptions, posts (only when a strategy they want to run needs them).

## Decide
| what you found | say / do next | read next |
|---|---|---|
| a batch or run is in progress | "Your batch on N phones is running: X done, Y left." Offer to watch it. | `goals/run-at-scale.md` |
| the newest run failed | "Your last run stopped at <plain step>. Want me to find out why?" | `goals/fix-a-failed-run.md` |
| total = 0 | "Let's connect your first iPhone." | `devices/connect-a-phone.md` |
| total > 0 but ready = 0 | "Your phone is not ready yet — let's fix the connection." | `devices/index.md` |
| an agent update is required | "The Titan PC Agent needs an update." (Runs still work; some phone maintenance actions are refused until it is updated.) | `devices/agent-update.md` |
| host dependencies need attention | "Your PC is missing something the phone connection needs." | `devices/host-deps.md` |
| no strategy visible | Ask: "What would you like your phones to do?" Then build one, or ask whether someone shared one with them. | `goals/build-a-strategy.md` |
| a strategy exists but its inputs (accounts, texts, posts) are missing | "To run it, I need your accounts — do you have a list? A file is fine." | `import/import-a-file.md` |
| everything ready | "Everything is ready. Shall I run <strategy> on <phone>?" | `goals/run-a-strategy.md` |

## How to say it
- One or two sentences, then the next step as a question or a single instruction.
- Name things the way the user sees them: the phone's name, the strategy's name, "your 12 TikTok accounts".
- Several problems at once: mention the most blocking one first; offer the rest after it is fixed.
