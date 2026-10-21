---
id: import/import-a-file
audience: both
intents: [import file, csv, upload list, add many accounts, here is my file, spreadsheet, text file]
tools: [list_resource_types, get_resource_type, list_resources, write_resources]
error_codes: [bulk_conflict, resource_timeout]
sources:
  - be:src/adapters/http/resources/batch_dto.rs
  - be:src/adapters/http/resources/batch_routes.rs
  - be:docs/business/features/shipped/F-RESOURCES.md
verified_be: 92a0a9c1
---
# The user gives you a file

Titan has no file import of its own — YOU read the file, turn each row into the right saved item, and write them in
one batch. The user should only see: what you found, a short preview, and the result.

1. **Read the file** (or the pasted text). Work out what it holds from its columns and values:
   | looks like | it is | page |
   |---|---|---|
   | usernames + passwords (maybe emails, platform) | social accounts | `import/accounts.md` |
   | one text per line or row | descriptions / captions | `import/descriptions.md` |
   | file names or folders of photos/videos with captions | posts | `import/posts.md` |
   | names + prompt texts | AI prompts | `import/prompts.md` |
   | network names + Wi-Fi passwords | Wi-Fi networks | `import/wifi-and-proxies.md` |
   | host + port (+ login) | proxies | `import/wifi-and-proxies.md` |
   Not sure? Ask one question: "Are these TikTok or Instagram accounts?"
2. **Check before writing**: `list_resource_types` → `get_resource_type` for the exact schema; validate every row
   against it; `list_resources` to spot rows that already exist (same platform + username for accounts).
3. **Show a plain preview** and ask once: "I found 40 TikTok accounts (38 new, 2 already saved — I'll skip those). 1 row
   has no password. Import the 38?" Show at most 3 example rows, never passwords.
4. **Write**: `write_resources` — up to 100 items per batch, each batch with its own new `idempotencyKey`; every item
   gets a new `resourceId` UUID you generate. A batch is all-or-nothing.
5. **Report**: "Imported 38 accounts. 1 row skipped (no password) — send it again when you have it."

If something goes wrong:
- No answer or `resource_timeout`: send the SAME batch with the SAME key again. Never a new key — that duplicates.
- `bulk_conflict`: nothing in that batch was saved. Re-read, remove the duplicate or stale row, send again.
- Never echo the passwords back, never keep the file's secrets in your reply.
