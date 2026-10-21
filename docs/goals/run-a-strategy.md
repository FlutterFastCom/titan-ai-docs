---
id: goals/run-a-strategy
audience: consumer
intents: [run strategy, start, run on my phone, use strategy, use shared strategy]
tools: [list_user_strategies, list_releases, get_release_run_inputs, get_release_input_options, list_user_presets, create_strategy_preset, list_devices, list_adapter_options, create_run, play_run, get_user_run, wait_seconds]
sources:
  - be:src/application/run_execution/create.rs
  - be:src/adapters/http/mcp/presentation.rs
verified_be: 92a0a9c1
---
# Run a strategy (own, shared or public)

1. Which strategy: `list_user_strategies`. More than one fits? Ask by name, not by ID.
2. Saved run settings for it? `list_user_presets` — reuse them if they match what the user wants.
3. Otherwise: `list_releases` → newest live release → `get_release_run_inputs` → for each choice
   `get_release_input_options`. Ask the user only for what is missing, in plain words ("Which account? Which post?").
   Missing content (no accounts saved yet) → `import/import-a-file.md`.
4. Phone: `list_devices` — a ready one; several ready → ask which, or offer a batch (`goals/run-at-scale.md`).
   AI service: `list_adapter_options` (if there is exactly one, use it without asking).
5. Confirm: "Run '<strategy>' on <phone> with <account>? It will post publicly." → `create_run`, then `play_run`
   (`build/5-run-it.md` has the exact shapes; for a shared/public strategy use the release form).
6. Watch with `get_user_run` + `wait_seconds`; report milestones and the result (`build/6-check-the-result.md`).
