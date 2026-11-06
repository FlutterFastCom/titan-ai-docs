---
id: build/5-run-it
audience: both
intents: [run settings, test run, start, run on phone]
tools: [list_releases, get_release_run_inputs, get_release_input_options, create_strategy_preset, list_devices, list_adapter_options, get_run_requirements, create_run, play_run]
error_codes: [run_create_operation_conflict, run_inputs_missing, invalid_run_inputs, run_args_invalid, resource_row_not_found, resource_schema_version_mismatch, resource_row_revision_conflict, resource_in_use, device_not_ready, supervision_required, supervision_unconfirmed, strategy_access_revoked, strategy_unavailable, run_already_running, run_not_playable]
---
# Step 5 — run settings and the first run

## Run settings
1. `list_releases` → pick the release (normally the newest live one) → `get_release_run_inputs` for its inputs.
2. For an input with choices (which account, which post): `get_release_input_options`.
3. Ask the user only for what is missing, in plain words ("Which account should post?").
4. `create_strategy_preset({body:{releaseId, name, inputs:[{inputId, value:{kind:"manual", value}} | {inputId,
   value:{kind:"resource", typeId, schemaVersion, rowId}}]}})` — `inputs:[]` when there are none. Keep the setup ID.

## Start one run
- Phone: `list_devices` — a phone that is ready. AI service: `list_adapter_options`.
- For the user's own setup: `get_run_requirements({query:{strategyPresetId}})`, then
  `create_run({body:{presetId, deviceUdid, llmAdapterId, argInvocation:{manual?, resources?, calls?}, manualMode:false}})`
  (`argInvocation` required, `{}` when empty; optional `wifiConfigId`).
- For a shared or public strategy the user cannot read inside: the release form
  `create_run({body:{strategyId, releaseId, inputs, deviceUdid, llmAdapterId}})`.
- Optional `operationId` on `create_run`: a fresh lowercase UUID you make up for ONE run intent. If the answer to a create
  is lost, send the SAME request with the SAME `operationId` and you get the original run back instead of a second run, for 24 hours; after that the same `operationId` counts as new.
  An identical request without one is also joined to the earlier run for about 2 minutes, or for as long as that run has not been started. The same `operationId` with a
  different request is refused (`run_create_operation_conflict`): use a new one for a genuinely new run. Make a new
  `operationId` for every deliberate extra run, or a second identical run will not be created.
- Then ALWAYS `play_run({path:{run_id}})` — creating does not start it. An empty answer means "accepted", not "done".
Confirm before starting anything that posts or changes accounts.

## Refusals (say the plain meaning, fix the cause)
| code | plain meaning → fix |
|---|---|
| `run_inputs_missing`, `invalid_run_inputs`, `run_args_invalid` | a run setting is missing or wrong → ask for / fix it |
| `resource_row_not_found`, `resource_schema_version_mismatch`, `resource_row_revision_conflict` | the chosen account/post changed or is gone → pick it again |
| `resource_in_use` | that account is busy in another run → wait or pick another |
| `device_not_ready` | the phone is not ready → `devices/index.md` |
| `app_not_installed`, `app_choice_required` (in the run's steps) | the app is missing, or two approved versions are installed → `devices/installed-apps.md` |
| `supervision_required`, `supervision_unconfirmed` | the phone needs supervision set up → `devices/index.md` |
| `strategy_access_revoked`, `strategy_unavailable` | the strategy is no longer available to them |
| `run_create_operation_conflict` | that `operationId` already belongs to a different request → use a new one for a different run |
| `run_already_running`, `run_not_playable` | read the run's real state instead of repeating |
