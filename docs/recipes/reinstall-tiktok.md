---
id: recipes/reinstall-tiktok
audience: author
intents: [reinstall tiktok, fresh tiktok, uninstall, install app, clean up]
tools: [list_tools, create_strategy, save_definition, create_release]
sources:
  - be:src/application/tools/dispatchable_catalog.rs
  - conductor:missions/2026-10-07-strategy-sharing-every-user-creates-prompt-parts/RELIABILITY-QUEUE.md
verified_be: 92a0a9c1
verified_conductor: 732df11a
---
# Recipe — remove TikTok and install it again   (each part PROVEN separately)

1. **Clean up** — AI steps: sign out / remove the saved login if wanted, then the uninstall tool, then a SEPARATE
   "is the app installed?" check (the uninstall answer alone does not prove it is gone).
2. **Install** — the install tool (it waits for the phone to confirm; one recorded install took ~13 minutes, so give
   the step a long time budget), then a separate "is the app installed?" check. An App Store sign-in or purchase prompt
   stops the step — tell the user.

Not proven: that cleanup erases every saved login from the phone's keychain or iCloud.

## Ask
"Which phone? Should saved logins be removed too? Is the phone signed into the App Store?"
