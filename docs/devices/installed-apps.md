---
id: devices/installed-apps
audience: both
intents: [which apps are on the phone, installed apps, app not installed, tiktok version, two tiktok, choose app variant, wrong app opened, app choice required, bundle]
tools: [list_device_apps, set_device_app_choice, device_action, get_device_screen, list_devices]
error_codes: [device_command_rejected, app_not_installed, app_choice_required, app_choice_invalid, invalid_device_apps_request, device_not_found]
---
# See what is installed on a phone and choose which version of an app to use

Some apps exist in more than one installed version on the same phone. TikTok is the usual case: the standard one
and a re-signed one are different apps to the phone. Titan only uses versions it approves, and a phone with two of them
needs one remembered choice.

## Look: `list_device_apps`
Takes the phone's UDID (`deviceId` from `list_devices`, never the numeric `rowId`). It asks the phone for a FRESH full
list: name, version and the exact app identifier for every app, system apps included, sorted by identifier. `observedAt`
is when Titan received the list. A failure is a refusal, never an empty list: an empty or missing answer is not proof an
app is absent. It counts toward the phone-command limit (`devices/control-the-phone.md`), is never cached, and takes no
extra input. Use it to learn the exact identifier before `device_action` `launch_app` / `close_app`.

## What launching does now
- `device_action` `launch_app` with an identifier: Titan reads the phone's list first. If that exact app is not there it
  refuses with `device_command_rejected` whose `diagnostic.cause` is `app_not_installed` (stage `inventory`) and sends
  nothing. A list that cannot be read surfaces as its own ordinary error (timeout, invalid result, phone session
  unavailable), never as absence and never as `inventory_unavailable` (that cause belongs to strategy steps).
  `app_not_installed` as the error code itself comes from `set_device_app_choice` and from strategy steps.
- A strategy's own launch step names the app by its Titan name (for example TikTok), not by identifier. It reads the
  phone's list, launches the one approved version that is installed, and its reply names the exact app it opened. With
  several approved versions installed and none chosen it stops with `app_choice_required` and sends nothing.
- A strategy's close-app step resolves the app the same way. `device_action` `close_app` does not check the list: it
  sends the identifier you give it.
- If the phone's list cannot be read, a strategy launch step tries each approved version once and its reply says it
  used the fallback; look at the screen to see which one opened.

## Fix `app_choice_required`
1. `list_device_apps` for that phone. Find the installed approved versions: the refusal also lists them as candidates.
2. Which one? Ask in plain words if you cannot tell: "TikTok is installed twice on this phone — the normal one and a
   second copy. Which one should Titan use?" Offer what the user used before as the default.
3. `set_device_app_choice({path:{device_id}, body:{appName:"TikTok", bundleId:"<one candidate, exactly as listed>"}})`.
   `appName` is the Titan app name; the identifier must be an approved version that a fresh list shows installed.
4. It remembers the choice for THAT phone only. It launches nothing and promises nothing about the next launch: start
   the run or action again and read what it opened.

## Refusals
| code | meaning → do |
|---|---|
| `app_not_installed` (or `device_command_rejected` with that cause) | the app is not on this phone. The refusal may list same-named apps that Titan does not approve; these never authorize anything. Ask the user to install the app on the phone, then `list_device_apps` again. You cannot install apps (`devices/control-the-phone.md`) |
| `app_choice_required` | several approved versions are installed, none chosen → the steps above |
| `app_choice_invalid` | the identifier you sent is not approved, was removed or changed since the list → `list_device_apps` again and send a listed candidate |
| `invalid_device_apps_request` | extra input, a body on the read, a malformed or unknown member, or an `appName` that is not a Titan catalog name or alias (use `TikTok`, never an identifier) → fix the call; nothing was changed |
| `device_not_found` | no phone has that identifier; a numeric row id lands here → use the UDID from `list_devices` |
| `app_catalog_unavailable` | Titan's app list is unavailable right now → wait, try once more, then `explain/device-and-app-codes.md` |
Other refusals (phone not ready, timeout, too many commands): `explain/device-and-app-codes.md` and `devices/index.md`.

## Talking to the user
Say apps by name ("TikTok"), never identifiers. After a choice: "Titan will use the second TikTok on this phone from
now on. Want me to start the run again?"
