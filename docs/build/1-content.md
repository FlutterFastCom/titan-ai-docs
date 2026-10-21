---
id: build/1-content
audience: author
intents: [save accounts, add descriptions, create post, resource types]
tools: [list_resource_types, get_resource_type, get_resource_type_version, list_resources, write_resources, create_wifi]
error_codes: [bulk_conflict, resource_timeout]
sources:
  - be:src/application/resources/records.rs
  - be:src/adapters/http/resources/batch_dto.rs
  - be:docs/business/features/shipped/F-RESOURCES.md
verified_be: 92a0a9c1
---
# Step 1 — the content the strategy will use

Most strategies need the user's accounts, texts or posts. If the user gives you a file, use `import/import-a-file.md`.
Otherwise ask for the values in plain words ("What should the profile description say?") and save them yourself.

## Find the right type
`list_resource_types` → pick the built-in type → `get_resource_type` (or `get_resource_type_version`) for its exact
`typeId`, `schemaVersion` and schema. Always use the returned schema; the summary below is only orientation.

| user's word | built-in type | required | notes |
|---|---|---|---|
| account | `account_emailset_v1` | `platform` (`tiktok` or `instagram`), `username`; secret `password` (1–500) | optional `email`, `otpSetup {mailboxId, recipient}` for login codes by email, `notes`, `externalId`. One per platform + username. |
| login-code mailbox | `email_mailbox_v1` | `email`, `emailProvider`, host/port overrides per schema | its passwords/tokens are secrets; create it before the account that points at it |
| login-code rule | `email_otp_rule_v1` | `name`, `rule` (sender domains, patterns, code length 4–8) | how to find the code inside a login email |
| description / caption text | the description type (one field) | `value`, 1–10,000 characters | |
| post | `post` | `name`, ordered `items` (1–9 to be postable) | each item: `file` (a media reference from the user's PC media library), `text`, `stickers`, `filter`; optional `description`, `music`, `location`, `privacy`, `coverIndex`, `allowComments`, `saveToDevice`, `aigcLabel` |
| AI prompt the user writes | `user_prompt` | `name`, `prompt`, `maxLength` (1–2,000) | optional `language` |
| Wi-Fi network | NOT a resource | use `create_wifi` (`name`, `ssid`, `security`, optional `password`, `hidden`, `autoJoin`) | |

## Save them — one batch
`write_resources` with a fresh `idempotencyKey` (a UUID you generate) and `body.items` = 1–100 items (max 2 MiB), all
committed together:
`{action:"create", resourceId:<new UUID you generate>, typeId, schemaVersion, data:{...}, secrets:{...}}`.
Replace: `{action:"replace", resourceId, expectedRevision, schemaVersion, data, secrets}` (complete objects, not a
patch). Delete: `{action:"delete", resourceId, expectedRevision}`.

- There is no dry-run. Check every row against the schema yourself first, and show the user a short plain preview
  before writing more than a few rows.
- More than 100 rows: several batches, each with its own key.
- No answer / `resource_timeout`: resend the SAME body with the SAME key — never a new key (that would duplicate).
- `bulk_conflict` (409): nothing was written. Read the current rows, fix the duplicate or stale row, send again.
- Never show a password or token back to the user.
