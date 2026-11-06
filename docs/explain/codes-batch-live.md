---
id: explain/codes-batch-live
audience: both
intents: [batch refused, queue error, batch busy, batch not found, item cannot be cancelled, phone cannot pause, batch unavailable, batch deleted, requeue refused]
tools: [get_queue_status, list_queues, list_queue_items, list_queue_workers, stop_queue, hard_pause_queue, requeue_queue_item, create_empty_queue_draft, list_devices, wait_seconds]
---
# Batch codes — a batch that exists (start, pause, stop, items, phones)

Say "batch", never "queue". Read `get_queue_status` before deciding what to do next. Setting-up codes:
`explain/codes-batch-drafts.md`.

| code | meaning | can you fix it | ask the user | only Titan |
|---|---|---|---|---|
| `run_queue_busy` | another action on this batch (start, pause, resume, stop, cancel, requeue, phone pause or resume) is still being processed | `wait_seconds` a few seconds, re-read `get_queue_status`, retry once | — | — |
| `run_queue_deleted` | the batch was deleted | no | "That batch was deleted. Shall I set up a new one?" | — |
| `run_queue_invalid_request` | a field or combination in the batch request is invalid | read `issues`, compare with the schema; no eligible per-item choice: use `create_run` + `play_run` | — | — |
| `run_queue_is_draft` | this is still a draft, so live-batch actions do not apply | edit the draft; finalize and start only if the user wants it to run | "It's still being set up. Shall I finish it and start it?" | — |
| `run_queue_not_draft` | that batch is no longer an editable draft | check its state with `list_queues` | — | — |
| `run_queue_not_found` | no batch with that id on this account | `list_queues` and pick the right one | — | — |
| `run_queue_status_conflict` | the batch cannot do that in its current state (for example resume a running one) | re-read `get_queue_status` and pick the action that fits | — | — |
| `run_queue_item_changed` | the item changed since you read it | re-read `list_queue_items`, decide again | — | — |
| `run_queue_item_not_found` | that item is not in the batch | re-read `list_queue_items` | — | — |
| `run_queue_item_not_cancellable` | the item cannot be cancelled in its state; a running item cannot be cancelled alone | re-read `list_queue_items`; only `stop_queue` or `hard_pause_queue` stop running work, and they affect the whole batch | "That run is already going; I can only stop the whole batch. Stop it?" | — |
| `run_queue_item_not_requeueable` | the item cannot be put back in the plan in its state | re-read `list_queue_items` | — | — |
| `run_queue_worker_not_found` | that phone is not part of this batch | re-read `list_queue_workers` | — | — |
| `run_queue_worker_status_conflict` | that phone cannot be paused or resumed in its current state | re-read `list_queue_workers` | — | — |
| `run_queue_worker_unavailable` | on start or resume: a phone chosen for the batch is not available; on requeue: none of the batch's original phones is available to take the work | no | "None of the phones in that batch is free or connected. Please check they're plugged in and the PC Agent is running." (`devices/index.md`) | — |
| `run_queue_unavailable` | Titan could not process the batch request just now | transient: wait, read state, retry once | — | if it persists |
