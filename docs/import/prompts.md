---
id: import/prompts
audience: both
intents: [import prompts, my prompt, ai prompt text, prompt list]
tools: [list_resource_types, get_resource_type, write_resources]
sources:
  - be:src/application/resources/records.rs
  - be:src/application/user_prompts/contract.rs
verified_be: 92a0a9c1
---
# Import the user's own AI prompts

Type `user_prompt`: `data.name` (up to 100 characters), `data.prompt` (up to 10,000 characters), `data.maxLength`
(integer 1–2,000 — the longest answer the prompt should produce; ask if the file has none, suggest 300), optional
`data.language`. `secrets:{}`.

These are prompts a strategy can use as an input (for example to generate captions). They are not the strategy's own
step instructions — those are built with `build/2-ai-steps.md`.
