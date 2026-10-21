---
id: import/descriptions
audience: both
intents: [import descriptions, captions, bios, texts, list of texts]
tools: [list_resource_types, get_resource_type, write_resources]
sources:
  - be:src/application/resources/records.rs
  - be:src/domain/descriptions.rs
verified_be: 92a0a9c1
---
# Import descriptions (captions, bios, any short texts)

The description type has one field: `data.value`, 1–10,000 characters, `secrets:{}`. Find its type with
`list_resource_types` (the built-in type whose schema is just `value`).

- One row / line / paragraph = one description. Ask once if the split is unclear: "Is each line a separate caption?"
- Keep emojis, hashtags and line breaks exactly as written.
- Skip empty rows; trim spaces at the ends only.
- Duplicates are allowed — mention them, don't drop them silently.
