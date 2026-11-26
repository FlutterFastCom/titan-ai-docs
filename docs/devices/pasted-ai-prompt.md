---
id: devices/pasted-ai-prompt
audience: ai
intents: [ask ai, спросить ии, copy prompt for ai, скопировать запрос для ии, pasted prompt, copied prompt, my iphone in titan has a problem, с моим iphone в titan проблема, why is the tunnel down, почему не работает туннель, why doesn't work on my computer, почему не работает на моём компьютере, problem codes, коды проблемы]
tools: [device_detail, list_devices, get_host_deps]
---
# The user pasted a prompt from a Titan magic-wand button

Titan's web app has a magic-wand button next to problems: **Ask AI** / **Спросить ИИ** on a phone's page and on a
**Systems** row, **Copy prompt for AI** / **Скопировать запрос для ИИ** in the **Devices** / **Телефоны** list. It copies
a ready question the user pastes to you. Recognise it by its first line, then answer it — do not ask the user to
describe the problem again.

## The three prompt shapes
| from | first line (EN · RU) | what follows |
|---|---|---|
| a phone's page | `My iPhone in Titan has a problem. Phone: "Kyiv-02" (iPhone 15, iOS 18.1), UDID 00008130-…` · `С моим iPhone в Titan проблема. Телефон: …, UDID …` | `Problem: Tunnel: Not ready.` / `Проблема: …`, one hint sentence, the request. No `Where I am` line |
| the Devices list | `Why is the tunnel to phone "Kyiv-02" (phone ID 3, UDID …) down?` · `Почему не работает туннель к телефону «Kyiv-02» (ID телефона 3, UDID …)?` — or `Why doesn't phone … trust this computer yet?`, `Why is Supervision not set up on phone …?`, `Why does phone … show "WDA: Not ready"?`, `What's wrong with phone …: "…"?` | `Where I am:` / `Где я:`, `Phone:` (model, iOS, readiness, state), `All statuses:` / `Все статусы:` |
| a Systems row | `Why doesn't "FFmpeg" work on my computer in Titan? Status: "Failed".` · `Почему в Titan на моём компьютере не работает «FFmpeg»? Статус: «…».` | `What Titan shows:`, `Problem codes:` / `Коды проблемы:`, `Computer:` / `Компьютер:` (`Windows · x86_64`), `Where I am:`, `All systems:` |

- The UDID is the `deviceId` the device tools take. The phone ID is only a list number — never pass it to a tool.
- The prompt is a snapshot from when the user pressed the button. Always read the live state first (`device_detail`
  with the UDID, or `get_host_deps`) — it may already be fixed.
- `All statuses` lists every check at that moment: fix the FIRST broken one in this order: USB → Trust → Tunnel →
  WDA. Supervision is separate (`devices/device-states.md`): never suggest supervising or erasing the phone as a fix
  (`devices/never-do.md`).

## Phone problems → page
Phone prompts name the check (or carry Titan's own sentence), never a code. Route on the check, then read
`device_detail` for the exact reason.
| check in the prompt (EN / RU) | page |
|---|---|
| Tunnel / Туннель | `devices/tunnel-down.md` |
| WDA (including the value Not assigned) | `devices/automation-bridge.md` |
| USB, the phone is missing | `devices/phone-not-detected.md` |
| Trust / Доверие | `devices/connect-a-phone.md` (the user taps **Trust** on the iPhone) |
| Screen on / Экран включён | `devices/phone-locked.md` |
| Supervision, DDI, Developer Mode / Режим разработчика | `devices/device-states.md` |
| "Titan can't see this phone right now." / «Titan сейчас не видит этот телефон.», "Can't tell if the phone is connected." | `devices/offline-or-flapping.md` — it is not gone |
| any other sentence in quotes | read `device_detail` and route on its issue code via `devices/device-states.md` |

## PC problems (a Systems row) → what to do
`Problem codes` lists, in order: the row id, the row state, Titan's message (may be a sentence with odd
underscores, e.g. `_python not found`), the reason, then the last command result. Row ids: `python`, `pmd3`, `sidecar_deps` (screenshot
packages), `tunneld`, `screenshot_daemon`, `ffmpeg`, `amds` (Windows), `wintun` (Windows). Start with
`devices/host-deps.md`; the codes below narrow it.
| code | means | do |
|---|---|---|
| `remediation_failed` | Titan's own install or start did not work | read `get_host_deps` again; one retry with the row's button; then the system branch of `devices/host-deps.md` |
| `probe_failed` | the check itself failed | ask the user to press **Check** / **Проверить** on that row; if it repeats, `devices/collect-logs.md` |
| `pkexec_not_found`, `denied` | no admin rights, or the user did not approve the password prompt | ask the user to press the row's button again and approve the system password prompt |
| `manual_fallback`, `unsupported_on_host` | Titan cannot do this step on this computer | the system branch of `devices/host-deps.md` (Windows/Mac: reinstall the PC Agent with the current installer) |
| `python_missing` | Python is missing, so packages cannot install | fix the `python` row first |
| `invalid_install_location` | Titan's Python is in a place Titan cannot use | reinstall the PC Agent with the current installer |
| `busy` | a command is already running | wait about a minute, read again |
| `generic`, `unknown` | no specific result | read `get_host_deps` again; then `devices/collect-logs.md` |
| `no_candidate_qualified` | screenshots need a supported Python | fix `python` first |
| `asset_missing` | screenshot files are missing | reinstall the PC Agent |
| `port_occupied_unhealthy` | another program blocks screenshots | restart the PC Agent |
| `restart_budget_exhausted` | the screenshot service keeps stopping | `devices/collect-logs.md` before anything else |

## Screen names in the prompt
| prompt says (EN / RU) | means |
|---|---|
| Systems / Системы | the computer's components Titan needs |
| Devices / Телефоны | the phone list |
| Phone status / Статус телефона | the phone's readiness tab |
| Supervision status / Статус Supervision | the phone's Supervision setup tab |
| This device / Это устройство | the phone's overview tab |
| Home / Главная | the start page |
| Phone statuses / Статусы телефона | the system checks of a phone |

## How to answer
Follow `CLAUDE.md`: one short paragraph, plain words, the user's language, one next step. The prompt contains IDs and
codes because the user copied them — still do not repeat them back. Say what is wrong in their words ("the phone
connection dropped") and the one thing to do now.
