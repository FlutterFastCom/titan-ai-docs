---
id: explain/codes-batch-drafts
audience: both
intents: [batch draft refused, queue draft error, clone batch, driver in use, phone in use, draft changed, operation conflict, reset required, options not available]
tools: [get_queue_draft, list_queue_draft_items, list_queue_draft_workers, get_queue_draft_defaults, set_queue_draft_default, edit_queue_draft, list_queue_draft_driver_options, list_queue_draft_argument_options, list_queue_draft_device_options, update_queue_draft_item_overrides, remove_queue_draft_items, remove_queue_draft_workers, clone_queue_as_draft, create_empty_queue_draft, get_queue_draft_edit_receipt, get_queue_draft_create_receipt, get_queue_draft_clone_receipt, list_queues, list_devices, wait_seconds]
---
# Batch codes — setting up a batch (drafts, copies, choices)

A "batch" is a set of runs across phones. Say "batch", never "queue" or "draft". After any of these, read the draft again
before retrying, and never repeat a request unchanged more than once. Live-batch codes: `explain/codes-batch-live.md`.

| code | meaning | can you fix it | ask the user | only Titan |
|---|---|---|---|---|
| `run_queue_draft_changed` | the draft changed since you read it | for a keyed edit read `get_queue_draft_edit_receipt` with the same operation id first (it may already be accepted); otherwise re-read `get_queue_draft`, reconcile, retry with a fresh operation id | — | — |
| `run_queue_draft_invalid_request` | a field or combination in the draft request is invalid | read `issues`, compare with the tool's schema; if the strategy has no eligible per-item choice, use `create_run` + `play_run` instead | — | — |
| `run_queue_draft_item_not_found` | that item is no longer in the draft | re-read `list_queue_draft_items` | — | — |
| `run_queue_draft_operation_conflict` | that operation id was already used for a different request | read the original's receipt (`get_queue_draft_create_receipt`, `…_edit_…`, `…_clone_…`); a changed request needs a fresh operation id | — | — |
| `run_queue_draft_operation_not_found` | no receipt exists for that operation id and action | check the id and that you used the matching receipt tool | — | — |
| `run_queue_draft_reset_required` | changing the strategy, its saved settings or the per-item choice would clear existing items and assignments, and the request did not allow a reset | after the user agrees, resend with the reset policy | "Changing this clears the items already in the batch. Go ahead?" | — |
| `run_queue_draft_exclusive_default_forbidden` | an account (something only one run may use at a time) cannot be a default for the whole batch | remove that default; set the account per item (`update_queue_draft_item_overrides`) or through the per-item choice | — | — |
| `run_queue_draft_driver_not_set` | option lists need the per-item choice (for example which accounts) set first | set it with `edit_queue_draft`, then list options | — | — |
| `run_queue_draft_driver_entity_unsupported` | Titan cannot list the per-item choices for this draft's kind of record | set each item's record directly with `update_queue_draft_item_overrides` (or in the item request) instead of picking from a list | — | — |
| `run_queue_draft_argument_entity_unsupported` | choices for that setting cannot be listed for this draft | set the value from the run settings instead of the option list | — | — |
| `run_queue_draft_argument_selector_is_driver` | you asked for options of the setting that decides one run per item; those are driver options | use `list_queue_draft_driver_options` | — | — |
| `run_queue_draft_argument_selector_not_found` | that setting does not exist on this draft | re-read `get_queue_draft` and use a setting it lists | — | — |
| `run_queue_driver_in_use` | a chosen account already drives an item in another running or paused batch | pick other accounts via `list_queue_draft_driver_options` | "Those accounts are busy in another batch. Use different ones, or wait until it finishes?" | — |
| `run_queue_item_entity_conflict` | some chosen values are not allowed in this draft | use the returned conflicts and `issues`, refresh the driver and argument options | — | — |
| `run_queue_item_entity_in_use` | a chosen account is held by another run right now | refresh the options (they exclude held ones), pick another or wait | "That account is busy in another run. Pick a different one, or wait?" | — |
| `run_queue_worker_assigned` | that phone still has a draft item assigned, so it cannot be removed | move or remove its items first, then `remove_queue_draft_workers` | — | — |
| `run_queue_worker_in_use` | a chosen phone is reserved by another ready, running or paused batch | pick another phone; or, with the user's yes, stop an abandoned ready batch to free it | "That phone is busy with another batch. Use a different one, or stop the other batch?" | — |
| `run_queue_worker_cooldown_conflict` | you asked to skip a phone's cooldown but it has none | nothing to do; re-read `list_queue_workers` | — | — |
| `run_queue_clone_source_changed` | the batch changed while you were copying it | re-read it, then `clone_queue_as_draft` again with a fresh operation id | — | — |
| `run_queue_clone_source_not_cloneable` | this batch cannot be copied: only a running or paused batch, or one that finished normally, can be copied (a stopped batch cannot) | check its state with `list_queues`; build a new draft with `create_empty_queue_draft` | — | — |
| `run_queue_clone_unsupported` | this batch cannot be turned into a draft copy (no reason is given) | build a new draft with `create_empty_queue_draft` instead | — | — |

Always-refused lookups: a missing AI service, run settings, proxy or Wi-Fi: `explain/codes-run-tools.md`.
