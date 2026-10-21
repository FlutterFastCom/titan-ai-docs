---
id: build/3-steps
audience: author
intents: [strategy steps, save strategy, loop, repeat, sub-strategy, call another strategy]
tools: [create_strategy, get_definition, save_definition, get_strategy_overview]
error_codes: [strategy_version_conflict, invalid_strategy_definition, invalid_strategy_input, preset_version_not_found, strategy_dependency_not_found, strategy_not_reusable, payload_too_large]
sources:
  - be:src/adapters/http/strategies/representations_definition.rs
  - be:src/application/strategies/definition.rs
  - be:src/adapters/http/auth_api_keys_catalog.rs
verified_be: 92a0a9c1
---
# Step 3 — create the strategy and save its steps

1. `create_strategy({body:{name, description?}})` → `strategyId`. Name 1–200 characters, description up to 2,000.
2. `get_definition` → note `versionId` (null for a new strategy).
3. `save_definition({path:{strategy_id}, body:{expectedVersionId, definition}})` — always the WHOLE tree.

## The tree — only three step kinds
```
root:  {kind:"group", name, arguments?, resourceBindings?, steps:[...]}
group: {kind:"group", name, defaultLoop?, steps:[...]}
llm:   {kind:"llm", name, defaultLoop?, promptVersionId, bindings, autoRetries, retryBackoffMs, irreversible, emailOtp?}
call:  {kind:"call", name, defaultLoop?, strategyId, strategyVersionId, bindings}
```
- `expectedVersionId`: null only the first time; afterwards the exact current `versionId` from `get_definition`.
- Omit `key` on new steps. Keep a returned `key` only when the step stays under the same parent.
- `bindings` maps a step's inputs to a literal or `{inherit:"<parent argument>"}`.
- Root `arguments`: name → `text` | `number` | `boolean`.
- `resourceBindings` (root only) maps a slot to saved content:
  `{<slot>:{typeId, schemaVersion, fields:[{argument, section:"data"|"secrets"|"state", path:[...]}], toolSelectors?}}`
  (`toolSelectors`: `otpAccount`, `publishPost`). This is how "which account" or "which post" becomes a run setting.
- `call` runs an exact saved version of another strategy; it must be released before this one can be.
- Login codes: an `llm` step can carry `emailOtp:{binding, path, ruleRef:{rowId, revision}}` pointing at a saved rule.

## Repeat, retry, wait
| need | use | limits |
|---|---|---|
| repeat a step or group | `defaultLoop:{times, delayMs?, maxDurationMs?}` | times 1–1000; delay fixed ms or `[min,max]` up to 1 day; duration up to 7 days |
| repeat the whole strategy | the loop in the run settings plan (not on the root) | |
| retry a flaky AI step | `autoRetries` 0–10 with `retryBackoffMs` (≤10 entries, each ≤1 h) | never on a step marked `irreversible` |
| an action that must not run twice (posting, reset confirm) | `irreversible:true` | the executor refuses to retry it |
| wait inside a step | the phone `wait` tool, 10 s per call | ask for several waits for longer pauses |

A loop counts as done when at least one pass succeeded — check every pass before telling the user "all N worked".

## Refusals
`strategy_version_conflict` → re-read, merge, save again · `invalid_strategy_definition` / `invalid_strategy_input` →
fix what the issue points at · `preset_version_not_found` / `strategy_dependency_not_found` → a pin is wrong or not
yours · `strategy_not_reusable` → the called strategy's owner must allow reuse · `payload_too_large` → simplify.
Limits: 500 steps, 256 pinned versions, nesting depth 8 (combined 16), 1 MiB.
