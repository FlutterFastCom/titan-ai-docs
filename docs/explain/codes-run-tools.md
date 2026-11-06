---
id: explain/codes-run-tools
audience: both
intents: [tool not enabled, unknown tool, tap offscreen, drag unconfirmed, run refused internally, irreversible step, ai service not found, proxy not found, wifi not found, settings not found, phone unplugged flag, record not installed]
tools: [list_adapters, list_adapter_options, list_user_presets, list_user_proxies, probe_user_proxy, list_wifis, list_resources, device_detail, list_devices, get_user_run, list_user_run_steps, read_run_transcript, get_device_screen, wait_seconds]
---
# Choices that do not exist, tool slips inside a run, and run-record faults

## A chosen thing does not exist (your own calls)
| code | meaning | can you fix it | ask the user | only Titan |
|---|---|---|---|---|
| `adapter_not_found` | the chosen AI service does not exist or is not the user's | pick one from `list_adapters` or `list_adapter_options` | none exists: "Please add an AI service in Titan first." | — |
| `preset_not_found` | the chosen saved run settings do not exist or are not the user's | pick from `list_user_presets` | — | — |
| `user_proxy_not_found` | the chosen proxy does not exist or is not the user's | pick from `list_user_proxies` | — | — |
| `user_proxy_probe_unavailable` | Titan could not check the proxy right now; this does NOT mean the proxy is dead | `wait_seconds`, then `probe_user_proxy` again | — | if it persists |
| `wifi_not_found` | the chosen Wi-Fi network does not exist or is not the user's | pick from `list_wifis` | — | — |
| `resource_type_not_installed` | a chosen record (account, post…) is of a kind that is not set up for this account (an old refusal) | pick a current record from `list_resources` and start again | — | — |

## Tool slips inside a run (what Titan's run AI got back)
You see these in `list_user_run_steps` and `read_run_transcript`. If the user owns the strategy, the cure is the step's
instructions (`build/7-improve-it.md`); for a shared strategy say "This needs a fix by the person who made the strategy."
| code | meaning | can you fix it | only Titan |
|---|---|---|---|
| `tool_not_enabled` | the step did not allow the tool its AI tried to use | own strategy: allow that tool in the step | — |
| `tool_requires_a11y` | the "list screen elements" tool always refuses (Titan has no accessibility access on the phone) | own strategy: change the instructions to use the screen picture | — |
| `unknown_tool` | the run's AI asked for a tool name that does not exist; usually a slip of that AI | own strategy and it repeats: tighten the instructions | only if it repeats with a tool Titan itself listed |
| `unimplemented_tool` | a tool Titan lists but cannot do (for example press Back: iPhones have none) | own strategy: change the step to use another way | yes if the step had no alternative |
| `tap_offscreen` | a tap was aimed outside the phone's screen | run again; if it repeats, fix the step's instructions | — |
| `continuous_drag_unconfirmed` | the PC Agent did not confirm a drag or swipe; it may or may not have happened | check `get_device_screen` (or the step screenshot) before running again | — |

## Phone flag
| code | meaning | can you fix it | ask the user |
|---|---|---|---|
| `presence_uncertain` | the PC Agent cannot currently confirm the phone is plugged in | `device_detail` (`devices/index.md`) | "Please check the phone's cable and that the PC Agent is running." |

## Titan-side failure (any call or tool)
| code | meaning |
|---|---|
| `internal_error` | something failed on Titan's side; it can come back as a tool reply inside a run or as a plain error on any call. It is not the user's doing: say "this is a problem on Titan's side, not yours", read the current state again before anything else, retry only a read, never repeat a write that may already have applied, and offer `devices/collect-logs.md` |

## Run-record faults (on a failed run, as its refusal code)
Titan's own bookkeeping of a run failed; none of these is the user's doing and none can be fixed with your tools. Say
plainly "this is a problem on Titan's side, not yours", do not retry around it, and offer `devices/collect-logs.md`.
| code | meaning |
|---|---|
| `invalid_attempt_evidence` | a step's record failed Titan's own consistency check |
| `invalid_leaf` | a step did not match the strategy version the run started with |
| `invalid_occurrence` | a step's position in the strategy was invalid |
| `invalid_outcome` | a step's result could not be recorded as reported |
| `invalid_owner` | a step's record did not belong to the run's owner |
| `replay_conflict` | a step was recorded twice with different contents |
| `chat_conflict` | Titan could not attach the run's AI conversation to the step |

`irreversible_policy`: Titan refused to repeat a step marked as one-time (for example a post): it gets one attempt. It is
also returned when the step's recorded result does not fit that rule, and the run does not say which. Never re-run it
blindly: first check whether it already posted ("That step can only run once; let me check whether it already posted
before trying again."). Only Titan can act in the second case.
