---
id: reference/tools-run-queues
generated: true
audience: both
---

# Tools — run-queues (8)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.

> A capitalised type name in an input (for example `QueueArgRequest`) is spelled out in request-shapes.md.


## preview_queue
- does: Preview run queue creation without starting work. Source-readable owners may read get_run_requirements with strategyPresetId or strategyPresetVersionId, copy root pickers[].selectorKey/scalars[].name or calls[]. scoped fields with the exact calls[].path, and echo strategyPresetVersionId into preview/create. Omit that pin to select the current saved preset at request time, resolved once. Labels are not selectors. Foreign-source release: on that exact runnable release, get_release_run_inputs returns each input's inputId, opaque queueArgument.selector/path (a nested resource slot can have path=[] because the selector encodes its location; never infer rootness from path) and the capability flags canBeDriver and canBeDraftDriver; get_release_input_options(inputId) returns pickable rowId values; use your own saved setup/version; get_run_requirements is only for source-readable owners, never a required first step. A direct queue needs exactly one input with canBeDriver=true (manual, entity or a ROOT resource slot); a nested resource slot reports canBeDriver=false and canBeDraftDriver=true, so it is draft-only. If no input has canBeDriver=true but one has canBeDraftDriver=true, use create_queue_draft; only if neither flag is present, including an inputless contract, use create_run then an explicit play_run; never invent a driver or dummy value.
- input: body PreviewRunQueueRequest { args: QueueArgRequest[]; llmAdapterId: integer; presetId: integer; strategyPresetVersionId?: string; userProxyId?: integer|null; wifiConfigId?: integer|null; workerDeviceRowIds: integer[] }
- returns: RunQueuePreviewResponse
- flags: read-only

## list_queues
- does: List the owner's run queues. Status may be draft, ready, active, paused, completed, stopped, or waiting; a waiting queue has requeued work with every phone released until an explicit resume_queue.
- input: query { page?: integer; size?: integer; status?: string; search?: string; orderBy?: string; orderDirection?: string; strategyId?: string }
- returns: PaginatedResponse_RunQueueListItemResponse
- flags: read-only

## list_queue_strategy_options
- does: List the distinct strategies (id and current name) selected by your own nondeleted run queues, including drafts without a pinned version; searchable by name substring or exact id, and paginated. Narrows your queue list only: it grants no strategy source or execution permission.
- input: query { page?: integer; size?: integer; search?: string }
- returns: PaginatedResponse_QueueStrategyOptionResponse
- flags: read-only

## create_and_start_queue
- does: Create and start an owned run queue in one call. `workerDeviceRowIds` are numeric `rowId` values from `list_devices`. Use `create_queue_draft` if the user wants to prepare work without starting it. Echo the captured strategyPresetVersionId; omission resolves the current saved setup once. Foreign-source release: on that exact runnable release, get_release_run_inputs returns each input's inputId, opaque queueArgument.selector/path (a nested resource slot can have path=[] because the selector encodes its location; never infer rootness from path) and the capability flags canBeDriver and canBeDraftDriver; get_release_input_options(inputId) returns pickable rowId values; use your own saved setup/version; get_run_requirements is only for source-readable owners, never a required first step. A direct queue needs exactly one input with canBeDriver=true (manual, entity or a ROOT resource slot); a nested resource slot reports canBeDriver=false and canBeDraftDriver=true, so it is draft-only. If no input has canBeDriver=true but one has canBeDraftDriver=true, use create_queue_draft; only if neither flag is present, including an inputless contract, use create_run then an explicit play_run; never invent a driver or dummy value.
- input: body CreateRunQueueSchema { args: QueueArgRequest[]; expectedItemCount: integer; llmAdapterId: integer; presetId: integer; strategyPresetVersionId?: string; userProxyId?: integer|null; wifiConfigId?: integer|null; workerDeviceRowIds: integer[] }
- returns: RunQueueCreateResponse
- flags: changes data · destructive: confirm with the user first

## create_queue_draft
- does: Create an owned run queue draft without starting work. `workerDeviceRowIds` are numeric `rowId` values from `list_devices`; after creation, use this draft’s scoped option tools for later choices. Add or edit items/workers/defaults, then read `revision` and finalize when ready. A lost response has no keyed receipt; inspect queues before retrying. The draft takes presetId and driver.selector (no version pin and no driver path member: a path inside driver is refused as an unknown field); it captures the current setup version, so read back the draft pin and revision. For a foreign-source release take the driver selector from an input with queueArgument.canBeDraftDriver=true in get_release_run_inputs and the driver values (row ids) from get_release_input_options for that inputId, and keep your own saved setup. Send only that input's opaque queueArgument.selector as driver.selector and never queueArgument.path: the server resolves the selector to its root or nested location, and a nested resource slot can show path=[]. get_run_requirements is only for source-readable owners. A draft needs exactly one input with canBeDraftDriver=true: an entity or resource slot, including a resource slot nested inside a called strategy. Only if no input has canBeDraftDriver=true, including an inputless contract, use create_run then an explicit play_run instead of inventing a driver.
- input: body CreateRunQueueDraftRequest { driver: DraftDriverRequest; presetId: integer; wifiConfigId?: integer|null; workerDeviceRowIds: integer[] }
- returns: DraftCreatedResponse
- flags: changes data
- refusals: 409: run_queue_worker_in_use

## get_queue_status
- does: Read the lifecycle of one owned run queue.
- input: path { queue_id: string }
- returns: DraftLifecycleResponse
- flags: read-only

## repair_queue_item_proxy
- does: Change the proxy binding of an eligible owned proxy-retired queue item. This changes the binding only; it does not probe the proxy, schedule the item or reopen the queue.
- input: path { queue_id: string; item_id: string } · body RepairProxyBindingRequest { binding: RecoveryBindingRequest; expectedBinding: RecoveryBindingRequest; expectedClaimGeneration: string }
- returns: ProxyBindingRepairedResponse
- flags: changes data · destructive: confirm with the user first

## reenqueue_queue_item
- does: For an owned item retired with `proxy_unavailable` and no linked run, probe its effective proxy and schedule it after recovery; a separate repair call is not required. Use this narrow recovery for that case. It only schedules: a completed queue becomes waiting and a waiting queue stays waiting, with every original worker still released and no drain notification. It never reacquires a phone or starts a queue; call resume_queue when execution is intended.
- input: path { queue_id: string; item_id: string } · body RetiredReenqueueRequest { expectedClaimGeneration: string; expectedEffectiveProxy: EffectiveProxyRequest }
- returns: RetiredReenqueueResponse
- flags: changes data · destructive: confirm with the user first
