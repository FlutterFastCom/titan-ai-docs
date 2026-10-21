---
id: build/2-ai-steps
audience: author
intents: [ai step, prompt, tool set, what can the phone do]
tools: [list_tools, get_tool, list_tool_sets, get_tool_set, get_tool_set_version, create_tool_set, get_prompt_part_lookups, create_prompt_part, create_prompt_part_version, create_prompt_part_preset, get_prompt_part_preset, list_prompt_part_presets]
error_codes: [preset_catalog_conflict]
sources:
  - be:src/adapters/http/mcp/presentation.rs
  - be:src/domain/tools/registry.rs
  - be:docs/business/features/shipped/F-PRODUCT-CATALOG.md
verified_be: 92a0a9c1
---
# Step 2 — the AI steps (what each step tells the phone-driving AI)

Every action on the phone happens inside an **AI step**: an AI looks at the screen and uses phone tools to reach the
step's goal. Each AI step needs: a **tool set** (which phone actions it may use), **prompt parts** (its instructions),
and one **prompt composition** that ties them together.

## 1. Pick the phone actions (tool set)
`list_tools` (enabled ones) → `get_tool` to read what each does. Reuse an existing tool set when one fits
(`list_tool_sets`, `get_tool_set_version`); otherwise `create_tool_set({body:{name, toolIds:[...]}})` (1–100 enabled tools,
in order) and keep the returned tool-set version ID. Useful phone tools that exist today include: launching apps,
typing text, waiting (max 10 s per call), waiting for an email login code, installing / uninstalling / checking an app,
connecting ExpressVPN to a location (`connect_vpn`), resetting network settings (`reset_network_settings`), sharing the
newest photos to TikTok (`share_photos_to_tiktok`), preparing post media, reading slide overlay text, and finishing a
task. Always confirm with `list_tools` — never assume one exists.

Give a step only the tools it needs: fewer tools, fewer wrong moves.

## 2. Write the instructions (prompt parts)
`get_prompt_part_lookups` lists the choices. Roles: `system` (who the AI is), `task` (what to achieve in this step),
`turn` (how to act each turn), `success_criteria` (what must be visibly true at the end), `postcheck` (how to double
check). `create_prompt_part({body:{name, role, body}})` creates the first version. If the text needs inputs, append a version
with `create_prompt_part_version({path:{part_id}, body:{..., argSignature:[{name, label, source:"manual", valueType, required}]}})` —
`valueType` is one of string, number, email, phone, secret, textarea. Inputs appear in text as `{{namespace.path}}`.

Writing good step instructions (from what worked on real phones):
- One clear goal per step ("Open the profile and set the bio to …"), with the exact end screen as success.
- Say what to do if the app is in an unexpected state (an old editor, a popup) — start from a known screen.
- Prove success from something only success shows (the account's own profile page, not a feed a guest also sees).
- Never ask the AI to "verify" by doing the action again.

## 3. Tie them together (prompt composition)
`create_prompt_part_preset({body:{name, initialVersion:{chatConfigAuthored:{postcheck:{enabled}, vision:{autoTrigger,
sparseThreshold}, budgets:{maxLlmTurns, maxDurationMs}}, parts:[{promptPartVersionId, orderIndex}], toolSetVersionId}}})`.
Keep its current version ID: that is the AI step's `promptVersionId`. On `preset_catalog_conflict`, re-read the
composition and reconcile — never overwrite blindly.

Every tool call takes only `path`, `query`, `body` and `idempotencyKey` at the top level; any other top-level field is
refused.
