---
id: explain/settings-and-navigation
audience: both
intents: [where is, how do I find, change language, create api key, settings, menu, navigation, app version]
tools: []
sources:
  - fe:src/pages/dashboard/DashboardPage.tsx
  - fe:src/widgets/AppSidebar/libs/constants/nav-items.constants.ts
  - fe:src/shared/i18n/locales/en/common.json
  - fe:src/shared/i18n/locales/ru/common.json
  - fe:src/shared/i18n/libs/constants/languages.constants.ts
verified_fe: a93e3f30
---
# Settings and where things are in the Titan web app

The web app is at https://app.titan.red (sign in with email and password).

None of these settings can be changed through your tools. Tell the user exactly where to click, using the labels below
(these are the English labels; in Russian the same items sit in the same places — their Russian names are in
`reference/ui-labels-ru.md`).

## The left menu
| menu group | item | what the user does there |
|---|---|---|
| — | Dashboard | a welcome page with shortcuts to Devices and Settings |
| Connection | PC Agent | connect the Titan PC Agent on their computer and see whether it is connected |
| Connection | Host Deps | see whether the PC has everything the phone connection needs, and run the fixes offered there |
| Connection | Devices | the list of iPhones and whether each is ready |
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
| User | email, access status and until when access is granted |
| Sessions | the user's signed-in sessions |
| API keys | create or revoke personal keys (see below) |
| Language | interface language: **English** or **Russian**. "Not set" means English. |
| Encoder | video encoder used when preparing videos: Auto, NVENC, VideoToolbox, Intel QSV, libx264 (CPU). Recommend **Auto** unless the user has a reason. |
| Encoder → Uniquization tier | how strongly videos are made unique before posting: Metadata only, Standard, TikTok Max, TikTok Max + Overlay |
| System | app and service versions |

Some sections may show "Save disabled — Titan cannot save this setting yet". Tell the user that setting cannot be
changed yet; do not suggest workarounds.

## Change the language
Say: "Open **Settings → Language**, pick English or Russian." The change applies to the web app only; you keep answering
in the language the user writes in. Russian names of every menu and button: `reference/ui-labels-ru.md`.

## Create an API key (needed to connect an AI like you)
Say:
1. "Open **Settings → API keys** and click **Create key**."
2. "Give it a name you will recognise, then click **Create key**."
3. "Copy the secret right away — it is shown only once."

Facts to mention only when relevant: at most 5 keys per account; a key can read everything the account can, including
saved Wi-Fi passwords, so keep it private; **Revoke** deletes a key at once and anything using it stops working.
Never ask the user to paste the key into the chat — it belongs in their AI tool's connection settings (see
`SETUP.md`).

## Which version is running
Say: "Open **Settings → System**." It shows the app version and the service version. Compare with `VERSION` in these
docs when the user reports something these docs do not describe.
