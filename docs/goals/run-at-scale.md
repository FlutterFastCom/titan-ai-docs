---
id: goals/run-at-scale
audience: both
intents: [many phones, all accounts, batch, repeat, schedule, every day, queue, run for each account]
tools: [preview_queue, create_and_start_queue, create_queue_draft, finalize_queue_draft, start_queue, list_queues, list_queue_items, list_queue_workers, pause_queue_worker, requeue_queue_item, resume_queue, soft_pause_queue, stop_queue]
---
# Run on many phones / for many accounts / repeatedly

## Ask
- Many accounts ⇒ a batch. Say this up front, and say one single test run goes first.
- Readiness = recent runs of the strategy; never say "not proven" to the customer. Read run settings first and offer them (VPN country etc.) as defaults; skip accounts whose last login failed; VPN is a run setting, a saved proxy is never a VPN.
- "Which phones?" (default: all ready ones) · "For which accounts or posts?" · "How many in total?"

## Do
1. Run settings must exist first (`goals/run-a-strategy.md` steps 1–3). Test once on one phone first (a single run, then the batch).
2. Read the run settings' current version ID and pass it as `strategyPresetVersionId` to BOTH the preview and the start
   — otherwise an edit in between changes what runs. `preview_queue` with the setup, AI service, phones (`workerDeviceRowIds` = the numeric `rowId` from
   `list_devices`) and the list that drives it (one run per account/post: `mode:"driver"` with `values`; same value
   every time: `fixed`; rotate a list across runs: `cycle`). It creates nothing — read its `itemCount`.
3. Say it plainly and confirm: "That's 24 runs across 3 phones, one per account. Start now?"
4. `create_and_start_queue` with the same body plus `expectedItemCount` — this STARTS it. Want to prepare without
   starting? `create_queue_draft` → … → `finalize_queue_draft` → `start_queue` later.
5. Watch: `list_queues`, `list_queue_items`, `list_queue_workers`; report progress every few minutes, not every poll.
6. Run failed items again (`requeue_queue_item`) — confirm with the user first, because it can lead to new posts:
   - batch still running (`active`): it stays active and the item runs again soon, by itself;
   - batch finished (`completed`): it becomes `waiting` with all its phones free and does NOT start by itself —
     `resume_queue` starts it again (confirm that too);
   - a `ready` or `paused` batch keeps its status.

## The `args` list — copy these shapes
`args` says what each run gets for each input of the strategy.
Every type is spelled out in `reference/request-shapes.md` (`QueueArgRequest`, `QueueValueRequest`).

**Where the `selector` comes from:**
- Your own strategy: `get_run_requirements` with `strategyPresetId` → `pickers[].selectorKey` (a picker) or `scalars[].name` (a typed value). Copy it exactly; a label is never a selector.
- Someone else's shared strategy: `get_release_run_inputs` → each input's `queueArgument.selector` and `queueArgument.path`; `queueArgument.canBeDriver` says whether it can drive a batch (`canBeDraftDriver` for a draft). Pickable row ids come from `get_release_input_options`.
- `path` is `[]` (or left out) for an input of the whole strategy.

**Exactly ONE entry is the driver** (`mode: "driver"`): one run per value in its `values`. It must be an input of the whole strategy (`path: []`).
A batch with no driver is refused, even when every other input is filled.
If the strategy has a single input (for example only a product description), that input IS the driver: list the same text once per run you want.
Every other input of the whole strategy must be listed too, as `fixed` (same value for every run) or `cycle` (rotate through a list) — even an optional one.

**Each value** is one of:
- typed text: `{ "kind": "manual", "value": "…" }`
- a picked row (account, post …): `{ "kind": "entity", "rowId": 123 }`
- a saved resource: `{ "kind": "resource", "typeId": "…", "schemaVersion": 1, "rowId": "…" }`. Build it from two
  reads — an option alone is NOT enough: `typeId` and `schemaVersion` come from that input in
  `get_release_run_inputs` (its `typeId` / `schemaVersion`), and `rowId` comes from the chosen option of
  `get_release_input_options` (an option is only `{rowId, label}`). Never send the option's `label`.

Two runs of a strategy whose only input is a product description, across two phones:
```json
{
  "presetId": 123,
  "strategyPresetVersionId": "<your run settings' version ID>",
  "llmAdapterId": 45,
  "workerDeviceRowIds": [11, 12],
  "args": [
    { "selector": "<selector>", "path": [], "mode": "driver", "values": [
      { "value": { "kind": "manual", "value": "<product description>" } },
      { "value": { "kind": "manual", "value": "<product description>" } }
    ] }
  ],
  "expectedItemCount": 2
}
```
`preview_queue` takes the same body without `expectedItemCount`; send the `itemCount` it returns as `expectedItemCount`.
Whichever phone is free takes the next run. To pin a run to one phone, add `"assignedWorkerDeviceRowId": 11` next to that run's `value`.

One run per account, with the same caption on every run:
```json
"args": [
  { "selector": "<account selector>", "mode": "driver", "values": [
    { "value": { "kind": "entity", "rowId": 501 } },
    { "value": { "kind": "entity", "rowId": 502 } }
  ] },
  { "selector": "<caption selector>", "mode": "fixed", "value": { "kind": "manual", "value": "<caption>" } }
]
```

A draft (`create_queue_draft`) takes `driver.selector` instead of `args`, and its driver must be a picked row or a saved resource, never typed text.
For a typed-text driver, use `create_and_start_queue`.

After a refusal, read the code in `explain/errors.md` and compare your body with these shapes.
Fix only what differs from them and send it once more; if it is refused again, stop and tell the user plainly what is refused — never guess further variants.

In the app the same batches live under **Runs and Monitor → Queues**.

## Limits to tell the user honestly
- A batch has no pause between runs and no "stop after the first failure": it keeps going until every run is done or
  you stop it. If the user needs either, say so before starting, and offer single runs one after another instead
  (with a wait between them and a check of each result).
- A batch needs one list that drives it; a strategy with no such input runs as single runs instead — never invent a
  dummy list.
- Repeats and pauses inside a run are supported (`build/3-steps.md`). A calendar schedule ("every day at 9:00") is NOT
  confirmed to exist — say so instead of promising it.
- Multiple accounts on one phone need a login step for each account — rotation without it posts from the same account.
