---
id: reference/tools-run-queues
generated: true
audience: both
---

# Tools — run-queues (8)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## preview_queue
- does: Preview run queue creation without starting work. Source-readable owners may read get_run_requirements with strategyPresetId or strategyPresetVersionId, copy root pickers[].selectorKey/scalars[].name or calls[]. scoped fields with the exact calls[].path, and echo strategyPresetVersionId into preview/create. Omit that pin to select the current saved preset at request time, resolved once. Labels are not selectors. Foreign-source release: on that exact runnable release, get_release_run_inputs returns each input's inputId and opaque queueArgument.selector/path (path=[] for root inputs) and get_release_input_options(inputId) returns pickable rowId values; use your own saved setup/version; get_run_requirements is only for source-readable owners, never a required first step. A queue needs exactly one eligible root driver (direct: manual/entity/resource root; draft: entity/resource only). If the release has no eligible root driver, including an inputless contract, a queue cannot be created: use create_run then an explicit play_run instead; never invent a driver or dummy value.
- input: body PreviewRunQueueRequest { args: QueueArgRequest[]; llmAdapterId: integer; presetId: integer; strategyPresetVersionId?: string; userProxyId?: integer|null; wifiConfigId?: integer|null; workerDeviceRowIds: integer[] }
- returns: RunQueuePreviewResponse
- flags: read-only

## list_queues
- does: List the owner's run queues.
- input: query { page?: integer; size?: integer; status?: string; search?: string; orderBy?: string; orderDirection?: string; strategyId?: string }
- returns: PaginatedResponse_RunQueueListItemResponse
- flags: read-only

## list_queue_strategy_options
- does: List the distinct strategies (id and current name) selected by your own nondeleted run queues, including drafts without a pinned version; searchable by name substring or exact id, and paginated. Narrows your queue list only: it grants no strategy source or execution permission.
- input: query { page?: integer; size?: integer; search?: string }
- returns: PaginatedResponse_QueueStrategyOptionResponse
- flags: read-only

## create_and_start_queue
- does: Create and start an owned run queue in one call. `workerDeviceRowIds` are numeric `rowId` values from `list_devices`. Use `create_queue_draft` if the user wants to prepare work without starting it. Echo the captured strategyPresetVersionId; omission resolves the current saved setup once. Foreign-source release: on that exact runnable release, get_release_run_inputs returns each input's inputId and opaque queueArgument.selector/path (path=[] for root inputs) and get_release_input_options(inputId) returns pickable rowId values; use your own saved setup/version; get_run_requirements is only for source-readable owners, never a required first step. A queue needs exactly one eligible root driver (direct: manual/entity/resource root; draft: entity/resource only). If the release has no eligible root driver, including an inputless contract, a queue cannot be created: use create_run then an explicit play_run instead; never invent a driver or dummy value.
- input: body CreateRunQueueSchema { args: QueueArgRequest[]; expectedItemCount: integer; llmAdapterId: integer; presetId: integer; strategyPresetVersionId?: string; userProxyId?: integer|null; wifiConfigId?: integer|null; workerDeviceRowIds: integer[] }
- returns: RunQueueCreateResponse
- flags: changes data · destructive: confirm with the user first

## create_queue_draft
- does: Create an owned run queue draft without starting work. `workerDeviceRowIds` are numeric `rowId` values from `list_devices`; after creation, use this draft’s scoped option tools for later choices. Add or edit items/workers/defaults, then read `revision` and finalize when ready. A lost response has no keyed receipt; inspect queues before retrying. The draft takes presetId and driver.selector (no version pin or driver path member); it captures the current setup version, so read back the draft pin and revision. For a foreign-source release take the root driver selector from get_release_run_inputs queueArgument.selector (root scope, path=[]) and the driver values (row ids) from get_release_input_options for that inputId, and keep your own saved setup; get_run_requirements is only for source-readable owners. Drafts accept only an entity/resource root driver; if the release has none, including an inputless contract, use create_run then an explicit play_run instead of inventing a driver.
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
- does: For an owned item retired with `proxy_unavailable` and no linked run, probe its effective proxy and schedule it after recovery; a separate repair call is not required. Use this narrow recovery for that case. It may reopen a completed queue to active, so work can resume.
- input: path { queue_id: string; item_id: string } · body RetiredReenqueueRequest { expectedClaimGeneration: string; expectedEffectiveProxy: EffectiveProxyRequest }
- returns: RetiredReenqueueResponse
- flags: changes data · destructive: confirm with the user first
