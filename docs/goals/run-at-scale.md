---
id: goals/run-at-scale
audience: both
intents: [many phones, all accounts, batch, repeat, schedule, every day, queue, run for each account]
tools: [preview_queue, create_and_start_queue, create_queue_draft, finalize_queue_draft, start_queue, list_queues, list_queue_items, list_queue_workers, pause_queue_worker, resume_queue, soft_pause_queue, stop_queue]
sources:
  - be:src/adapters/http/run_execution/queue_dto.rs
  - be:src/application/run_execution/queue/generator.rs
  - be:src/adapters/http/auth_api_keys_catalog.rs
verified_be: 92a0a9c1
---
# Run on many phones / for many accounts / repeatedly

## Ask
- "Which phones?" (default: all ready ones) · "For which accounts or posts?" · "How many in total?"

## Do
1. Run settings must exist first (`goals/run-a-strategy.md` steps 1–3). Test once on one phone if never run before.
2. Read the run settings' current version ID and pass it as `strategyPresetVersionId` to BOTH the preview and the start
   — otherwise an edit in between changes what runs. `preview_queue` with the setup, AI service, phones (`workerDeviceRowIds` = the numeric `rowId` from
   `list_devices`) and the list that drives it (one run per account/post: `mode:"driver"` with `values`; same value
   every time: `fixed`; rotate a list across runs: `cycle`). It creates nothing — read its `itemCount`.
3. Say it plainly and confirm: "That's 24 runs across 3 phones, one per account. Start now?"
4. `create_and_start_queue` with the same body plus `expectedItemCount` — this STARTS it. Want to prepare without
   starting? `create_queue_draft` → … → `finalize_queue_draft` → `start_queue` later.
5. Watch: `list_queues`, `list_queue_items`, `list_queue_workers`; report progress every few minutes, not every poll.

## Limits to tell the user honestly
- A batch has no pause between runs and no "stop after the first failure": it keeps going until every run is done or
  you stop it. If the user needs either, say so before starting, and offer single runs one after another instead
  (with a wait between them and a check of each result).
- A batch needs one list that drives it; a strategy with no such input runs as single runs instead — never invent a
  dummy list.
- Repeats and pauses inside a run are supported (`build/3-steps.md`). A calendar schedule ("every day at 9:00") is NOT
  confirmed to exist — say so instead of promising it.
- Multiple accounts on one phone need a login step for each account — rotation without it posts from the same account.
