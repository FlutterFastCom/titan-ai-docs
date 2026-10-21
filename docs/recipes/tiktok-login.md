---
id: recipes/tiktok-login
audience: author
intents: [login tiktok, sign in account, login with email code, otp]
tools: [write_resources, create_strategy, save_definition, create_release]
sources:
  - conductor:missions/2026-10-15-reusable-email-otp-account-types-carry-an-otpset/LOGIN-QUEUE.md
  - be:src/application/tools/registry.rs
  - be:src/adapters/http/strategies/email_otp.rs
verified_be: 92a0a9c1
verified_conductor: 732df11a
---
# Recipe — log into a TikTok account   (PROVEN)

Keep login as its own sub-strategy so other strategies can call it before posting.

## Steps (AI steps)
1. Open TikTok and go to log in / add account (start from TikTok's first login screen, not a half-finished
   verification screen).
2. Enter the account's username and password; when TikTok asks for an email code, the step waits for it with the
   email-code tool and types it in.
3. Prove success on the account's OWN profile page (a logged-in feed proves nothing — guests see one too). Do not log
   out again at the end (an early prompt did, and undid the login).

## What it needs
A saved account with an email login-code setup (`import/accounts.md`, "Login codes by email") and a saved login-code
rule; the AI step carries `emailOtp` pointing at that rule (`build/3-steps.md`). The account's mailbox may need the
user's consent for this release: `get_email_otp_consents` / `set_email_otp_consent` before the first run.

## Ask
"Which account? Should Titan read the login code from that account's email inbox?"
