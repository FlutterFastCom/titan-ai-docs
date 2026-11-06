---
id: explain/codes-run-email
audience: both
intents: [login code did not arrive, email code, otp, login link, mailbox not reachable, mailbox refused, email account not connected]
tools: [list_resources, get_run_requirements, read_run_transcript, get_user_message_screenshot, wait_seconds]
---
# Email-code and login-link codes inside a run

These are what Titan's run AI got back while reading a mailbox for a login code or link. You see them in a run's steps
(`list_user_run_steps`) and transcript (`read_run_transcript`, owned strategies only); you cannot call those tools
yourself. Fix the cause, then run again. Never echo a mailbox password or code.

| code | meaning | can you fix it | ask the user | only Titan |
|---|---|---|---|---|
| `otp_inbox_unconfigured`, `login_link_inbox_unconfigured` | the account's mailbox is not set up enough to read mail (address, server or sign-in details missing), or the account's platform does not match the strategy's email rule | run once more first (the same code appears when the mailbox changed while Titan was waiting); if it repeats, check the account with `list_resources` and fill in what is missing (`import/accounts.md`) | "The email for this account isn't connected yet. Let's add its mailbox details." | — |
| `otp_reauth_required`, `login_link_reauth_required` | the mailbox's sign-in authorization expired | no | "Titan can't sign in to this account's mailbox any more. The email password or access needs to be updated." | — |
| `otp_inbox_unreachable`, `login_link_inbox_unreachable` | the mailbox could not be reached or read right now | no: run again later | "The mailbox didn't answer. I'll try the run again in a bit." | if it repeats for several accounts |
| `otp_await_timeout`, `login_link_await_timeout` | no code or link arrived within the wait | look at the run's screenshot (`get_user_message_screenshot`) for why the app did not send one, then run again | "No code arrived in the email this time. I'll check what the phone showed and try again." | — |
| `otp_missing_account_binding`, `login_link_missing_account_binding`, `open_url_missing_account_binding` | the call had no email account attached; every run turn attaches one, so this is not expected | no | — | yes: a Titan-side fault, tell the user plainly and offer `devices/collect-logs.md` |
