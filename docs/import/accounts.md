---
id: import/accounts
audience: both
intents: [import accounts, add tiktok accounts, add instagram accounts, account list]
tools: [list_resource_types, get_resource_type, list_resources, write_resources]
sources:
  - be:src/application/resources/records.rs
  - be:src/application/resources/mailboxes.rs
  - be:src/domain/accounts.rs
verified_be: 92a0a9c1
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
That needs a **mailbox** (`email_mailbox_v1`: the mailbox address, provider, its password or app password as a secret)
and, on the account, `data.otpSetup:{mailboxId, recipient}`. Put the mailbox item BEFORE the account in the same
batch, and use the mailbox's new `resourceId` as `mailboxId`. Ask: "Should Titan read the login codes from these
email inboxes? Then I also need each inbox's password or app password."
