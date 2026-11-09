---
id: explain/settings-and-navigation
audience: both
intents: [where is, how do I find, change language, create api key, settings, menu, navigation, app version, forgot password, reset password, can't sign in, confirm email, verification email, sign up]
tools: []
---
# Settings and where things are in the Titan web app

The web app is at https://app.titan.red (sign in with email and password).

None of these settings can be changed through your tools. Tell the user exactly where to click, using the labels below
(these are the English labels; in Russian the same items sit in the same places — their Russian names are in
`reference/ui-labels-ru.md`).

## The left menu
| menu group | item | what the user does there |
|---|---|---|
| — | Dashboard | the home page: whether the PC Agent is connected, the phone counts (ready / pending / unknown), and the active batches |
| Connection | PC Agent | connect the Titan PC Agent on their computer and see whether it is connected |
| Connection | Host Deps | see whether the PC has everything the phone connection needs, and run the fixes offered there |
| Connection | Devices | the list of iPhones and whether each is ready; open a phone and click **Rename** to give it a name they recognise |
| Runs and Monitor | Queues | batches of runs across several phones |
| Runs and Monitor | My Runs | every run, its progress and its result |
| Runs and Monitor | Strategies | the strategies they own, that were shared with them, or that are public |
| Settings and Setup | Resources | accounts, descriptions, posts and other saved content |
| Settings and Setup | Proxies | saved proxies |
| Settings and Setup | WiFi | saved Wi-Fi networks |
| Settings and Setup | AI Adapters | which AI service powers the AI steps |
| Settings and Setup | Settings | personal settings (below) |

Administrators also see a Dev Tools group (Playground, LLM Templates). Never send a regular user there.

## Settings page sections
| section | contains |
|---|---|
| User | nickname (**Change nickname**), email, access status and until when access is granted; **Password** — **Email me a reset link**; under **Technical details** the User ID (with a copy button); **Support access** — a checkbox that lets Titan support view their data to help with problems |
| Invites | invite links for new people (each works once and expires after 7 days; an administrator sets how many) |
| Sessions | the user's signed-in sessions |
| MCP API keys | create or revoke the keys that connect an AI assistant like you (see below) |
| Language | interface language: **English** or **Russian**. "Not set" means English. |
| Encoder | video encoder used when preparing videos: Auto, NVENC, VideoToolbox, Intel QSV, libx264 (CPU). Recommend **Auto** unless the user has a reason. |
| Encoder → Uniquization tier | how strongly videos are made unique before posting: Metadata only, Standard, TikTok Max, TikTok Max + Overlay |
| System | app and service versions |

Some sections may show "Save disabled — Titan cannot save this setting yet". Tell the user that setting cannot be
changed yet; do not suggest workarounds.

## Signing in, a new account, a forgotten password
You cannot do any of this for the user — it happens in their browser and their email. Tell them the steps.

**New account.** On the sign-in page, the **Register** tab: email, nickname, **Password** and **Confirm password**
(the same password twice, at least 8 characters), invite code. After **Register** they must confirm the email before
they can sign in: "Open the email from Titan and open its link. On the page that opens, press **Confirm email**, then
sign in." Opening the link alone does not confirm the account. Confirmation links work once, for 24 hours.

**"Confirm your email" when signing in.** The account was never confirmed. Say: "Open the confirmation email from
Titan (check spam) and confirm it. If you can't find it, press **Send verification email** on that screen." After a
send the button reads **Send again** and waits at least a minute before the next one.

**Forgot the password.** Say:
1. "On the sign-in page, press **Forgot password?**, enter your email, press **Send reset link**."
2. "Open the email from Titan and press its **Reset password** button. Enter the new password twice and press
   **Change password**."
3. "Sign in with the new password." Changing it signs the user out on all devices, this one included.

A reset link works once, for 60 minutes; only the newest email's link works. "This reset link is no longer valid" =
press **Request a new link**. After **Send reset link** the screen says "If an account exists for that email, we sent a
reset link" — it never tells whether the account exists. If nothing arrives, check spam, then check the email address.

**Signed in and wants a new password:** **Settings → User → Password → Email me a reset link**, then **Send reset link**
in the box that opens, then steps 2–3. They stay signed in until they set the new password.

Changing the password does NOT disconnect you: MCP API keys keep working. Never ask the user for their password.

## Change the language
Say: "Open **Settings → Language**, pick English or Russian." The change applies to the web app only; you keep answering
in the language the user writes in. Russian names of every menu and button: `reference/ui-labels-ru.md`.

## Create an API key (needed to connect an AI like you)
Say:
1. "Open **Settings → MCP API keys** and click **Create key**."
2. "Give it a name you will recognise, then click **Create key**."
3. "Copy the secret right away — it is shown only once."

Facts to mention only when relevant: at most 5 keys per account; a key reaches only the actions published for keys (the
MCP tools) — some account pages, such as the Settings page itself, answer a key with `api_key_route_forbidden` — but
what it can read includes saved Wi-Fi passwords, so keep it private; **Revoke** deletes a key at once and anything using it stops working.
Never ask the user to paste the key into the chat — it belongs in their AI tool's connection settings (see
`SETUP.md`).

## Which version is running
Say: "Open **Settings → System**." It shows the app version and the service version. Compare with `VERSION` in these
docs when the user reports something these docs do not describe.
