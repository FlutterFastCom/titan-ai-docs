---
id: import/accounts
audience: both
intents: [import accounts, add tiktok accounts, add instagram accounts, account list]
tools: [list_resource_types, get_resource_type, list_resources, write_resources]
---
# Import social accounts

Type: `account_emailset_v1` (confirm with `get_resource_type`).

| file column (any name) | goes to | rule |
|---|---|---|
| platform | `data.platform` | `tiktok` or `instagram`; if the file has none, ask once for the whole file |
| username / login / handle | `data.username` | required, up to 200 characters, without a leading @ unless the user wants it |
| password | `secrets.password` | required, 1–500 characters |
| email | `data.email` | optional, up to 320 |
| notes / comment | `data.notes` | optional, up to 10,000 |
| id / external id | `data.externalId` | optional, up to 200 |

One account per platform + username: existing ones are duplicates — skip them, or replace them only if the user asks
(`action:"replace"` with the row's current `expectedRevision`).

## Login codes by email (only if the user wants Titan to read the login code)
That needs a **mailbox** (`email_mailbox_v1`: the mailbox address, provider, and its sign-in secret) and, on the
account, `data.otpSetup:{mailboxId, recipient}`. Put the mailbox item BEFORE the account in the same batch, and use the
mailbox's new `resourceId` as `mailboxId`.

**Which mailboxes work with a password.** Titan reads Gmail, Outlook/Hotmail/Live (Microsoft) and Yahoo inboxes ONLY
through OAuth — a password or app password for these providers is saved without error but can never be used: every run
that needs the login code is refused with `invalid_resource_binding`. Do not create such a mailbox from a password.
Tell the user: "Titan can't read Gmail / Outlook / Yahoo codes with a password. Titan support has to connect that
inbox for you." Password (or app-password) sign-in works for the other providers (Mail.ru, Yandex, GMX, WEB.DE) and
for a custom IMAP server. For those, ask: "Should Titan read the login codes from these email inboxes? Then I also
need each inbox's password or app password."
