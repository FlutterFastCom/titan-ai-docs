---
id: devices/control-the-phone
audience: both
intents: [see the phone screen, what is on the phone, tap on the phone, phone stuck, run stuck, unstick the phone, press home, open app, close app, dismiss popup]
tools: [get_device_screen, device_action, device_detail, list_devices, wait_seconds]
error_codes: [device_command_rate_limited, device_busy]
sources:
  - be:src/adapters/http/mcp/presentation.rs
  - be:src/adapters/http/auth_api_keys_catalog.rs
  - be:src/application/commands/phone_control.rs
  - be:src/domain/device_commands/phone_control.rs
verified_be: 98be5af0
---
# See the phone's screen and act on it

Two tools let you look at one of the user's iPhones right now and do one simple thing on it. Use them to see why a
phone or a run is stuck and to get it back to a normal screen. Both take the phone's UDID (`deviceId` from
`list_devices`), not the numeric `rowId`. They need the phone connected with a working automation link: if they
refuse because the phone isn't ready, follow `devices/index.md` instead.

## Look: `get_device_screen`
Takes a NEW screenshot and returns it as an image with its `width` and `height`. `receivedAt` is when Titan's server
received this screenshot; use it as the screenshot's time (the phone's own capture time is not given). If no fresh
picture can be taken the call refuses — it never gives you an old one. Do not use the cached thumbnail tools to judge
what the phone shows now.

## Act: `device_action` (one action per call)
| `action.type` | what it does | fields |
|---|---|---|
| `tap_pct` | tap a point | `x_pct`, `y_pct` |
| `long_press_pct` | press and hold | `x_pct`, `y_pct`, optional `duration_ms` |
| `swipe_pct` | swipe from one point to another | `from_x_pct`, `from_y_pct`, `to_x_pct`, `to_y_pct`, optional `duration_ms` |
| `scroll_pct` | scroll the content | `direction` (up/down/left/right), `distance_pct` (above 0, at most 0.8), optional `duration_ms` |
| `press_home` | go to the Home Screen | — |
| `launch_app` / `close_app` | open / close an app | `bundle_id` (e.g. `com.zhiliaoapp.musically` for TikTok) |
| `accept_alert` / `dismiss_alert` | answer the system popup on screen | — |
| `type_text` | type into the field that has focus | `text` |
Points are fractions of the screen: 0,0 is top-left, 1,1 is bottom-right. Work them out from the screenshot's width
and height. Durations are milliseconds.

## The rules
1. **Look first, every time.** Take a screenshot before the first action, and read the screenshot that comes back with
   each action before the next one. The reply only says the action was delivered; whether it worked is unknown until
   you see it. If the reply has no screenshot (`screen` is null, `screenError` set), the action still went through:
   take a new screenshot before doing anything else.
2. **Never repeat an action automatically.** Each action is sent once and never retried. After a timeout or an unclear
   answer, look at a new screenshot instead of sending it again — a second "Post" tap could post twice.
3. **It works during a run.** A running strategy may be using the same phone; your action and the run's actions can mix.
   Use it to free a stuck run (close a popup, go Home), not while a run is working normally. Actions on one phone are
   carried out one at a time.
4. **Limits.** Screenshots and actions share the user's phone-command limit. On `device_command_rate_limited`, wait
   the time it says (`retryAfterSeconds`) with `wait_seconds`, then look again. `device_busy` means the phone is busy
   with another command and gives no wait time: wait a few seconds, take a new screenshot, then decide. A refused
   call (either code) was NOT sent, so sending it after that is not a repeat.
5. **Not possible here:** a Back button (iPhones have none — tap the app's own back arrow), opening links or web
   addresses, installing or deleting apps, clearing app data, network reset, VPN, Wi-Fi passwords, erasing the phone.
   Don't promise these; strategies do the bigger jobs.

## Talking to the user
- Say what you see and do in plain words: "TikTok is showing a 'Save draft?' popup. I'll close it and go back to the
  Home Screen." Never say tap coordinates, tool names or bundle IDs.
- **Ask first** before anything that acts in the outside world or can't be undone: typing into or tapping Post / Send /
  Delete / Log out in a social app, accepting a permission or sign-in popup. `device_action` is
  marked destructive, so before your first action on a phone, say the plan in one sentence and wait for yes:
  "I'll close TikTok and reopen it to get the phone unstuck. OK?" One yes covers that described plan; anything beyond
  it needs a new yes.
- When the run was in the middle of posting, ask before touching the phone at all — check first whether it already
  posted (`goals/fix-a-failed-run.md`).
- NEVER type a password, the phone's passcode or a one-time login code with `type_text`, even if the user agrees —
  the user enters those on the phone themselves. Never repeat one back. Other text (a search term, a caption the user
  approved) is fine.
- Finished: tell the user what the screen shows now ("TikTok is open on the home feed again"), then the next step.
