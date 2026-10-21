---
id: reference/tools-run-queue-drafts
generated: true
audience: both
---

# Tools — run-queue-drafts (21)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## delete_queue_drafts
- does: Destructive: tombstone a batch of owned run queue drafts.
- input: body RunQueueDraftsBatchDeleteRequest { queueIds: string[] }
- returns: RunQueueDraftsBatchDeleteResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 503: auth_profile_unavailable, run_queue_unavailable

## create_empty_queue_draft
- does: Create one blank progressive run queue draft keyed by a fresh canonical UUID in `operation_id`; the body is exactly `{}`. This is not the initialized `create_queue_draft`, which takes required initial values and has no keyed receipt. Retain the operation_id: after an uncertain response read get_queue_draft_create_receipt with the same operation_id or retry the identical PUT; a changed request needs a fresh operation_id.
- input: path { operation_id: string } · body CreateEmptyQueueDraftRequest {  }
- returns: DraftOperationResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_operation_not_found, run_queue_not_found, preset_not_found, strategy_not_found, adapter_not_found, user_proxy_not_found, wifi_not_found, device_not_found, resource_row_not_found · 409: run_queue_draft_operation_conflict, run_queue_draft_changed, run_queue_not_draft, run_queue_draft_reset_required, run_queue_worker_in_use, run_queue_driver_in_use, run_queue_clone_source_not_cloneable, run_queue_clone_source_changed, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 413: payload_too_large · 422: run_queue_draft_incomplete, run_queue_clone_unsupported, publish_post_invalid, invalid_resource_binding, run_inputs_missing · 503: auth_profile_unavailable, run_queue_unavailable

## get_queue_draft_create_receipt
- does: Read the owner-scoped receipt of a `create_empty_queue_draft` operation by its canonical `operation_id`; missing, foreign and wrong-action receipts remain masked. A read never applies an operation.
- input: path { operation_id: string }
- returns: DraftOperationResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_operation_not_found, run_queue_not_found · 503: auth_profile_unavailable, run_queue_unavailable

## edit_queue_draft
- does: Destructive: edit an existing owned draft keyed by a fresh canonical UUID in `operation_id`, with `queueId` and `expectedRevision` (no `kind` member); omit `driver` to keep it. A driver replacement supplies an entity-field `selector` and the complete desired `items` list; for each entity-field driver item supply an explicit `{kind:"entity",rowId:"..."}` value. Retain the operation_id: after an uncertain response read get_queue_draft_edit_receipt with the same operation_id or retry the identical PUT; a changed request needs a fresh operation_id.
- input: path { operation_id: string } · body EditQueueDraftRequest { driver?: null | EditDriverRequest; expectedRevision: string; incompatibleAssignmentPolicy?: "reject"|"reset"; queueId: string; setup?: null | EditSetupRequest }
- returns: DraftOperationResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_operation_not_found, run_queue_not_found, preset_not_found, strategy_not_found, adapter_not_found, user_proxy_not_found, wifi_not_found, device_not_found, resource_row_not_found · 409: run_queue_draft_operation_conflict, run_queue_draft_changed, run_queue_not_draft, run_queue_draft_reset_required, run_queue_worker_in_use, run_queue_driver_in_use, run_queue_clone_source_not_cloneable, run_queue_clone_source_changed, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 413: payload_too_large · 422: run_queue_draft_incomplete, run_queue_clone_unsupported, publish_post_invalid, invalid_resource_binding, run_inputs_missing · 503: auth_profile_unavailable, run_queue_unavailable

## get_queue_draft_edit_receipt
- does: Read the owner-scoped receipt of an `edit_queue_draft` operation by its canonical `operation_id`; missing, foreign and wrong-action receipts remain masked. A read never applies an operation.
- input: path { operation_id: string }
- returns: DraftOperationResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_operation_not_found, run_queue_not_found · 503: auth_profile_unavailable, run_queue_unavailable

## clone_queue_as_draft
- does: Clone an owned queue as a new draft keyed by a fresh canonical UUID in `operation_id`; the body is exactly `{sourceQueueId}`. Active, paused or completed sources are accepted; draft/ready and stopped sources are refused by different lifecycle codes. Retain the operation_id: after an uncertain response read get_queue_draft_clone_receipt with the same operation_id or retry the identical PUT; a changed request needs a fresh operation_id.
- input: path { operation_id: string } · body CloneQueueAsDraftRequest { sourceQueueId: string }
- returns: DraftOperationResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_operation_not_found, run_queue_not_found, preset_not_found, strategy_not_found, adapter_not_found, user_proxy_not_found, wifi_not_found, device_not_found, resource_row_not_found · 409: run_queue_draft_operation_conflict, run_queue_draft_changed, run_queue_not_draft, run_queue_draft_reset_required, run_queue_worker_in_use, run_queue_driver_in_use, run_queue_clone_source_not_cloneable, run_queue_clone_source_changed, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 413: payload_too_large · 422: run_queue_draft_incomplete, run_queue_clone_unsupported, publish_post_invalid, invalid_resource_binding, run_inputs_missing · 503: auth_profile_unavailable, run_queue_unavailable

## get_queue_draft_clone_receipt
- does: Read the owner-scoped receipt of a `clone_queue_as_draft` operation by its canonical `operation_id`; missing, foreign and wrong-action receipts remain masked. A read never applies an operation.
- input: path { operation_id: string }
- returns: DraftOperationResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_operation_not_found, run_queue_not_found · 503: auth_profile_unavailable, run_queue_unavailable

## list_queue_draft_argument_options
- does: List current argument candidates for an owned draft by query.queueId and selectorKey; use this instead of global resource lists for later draft argument choices.
- input: query { queueId: string; selectorKey: string; page?: string; size?: string; search?: string }
- returns: QueueOptionsResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request, invalid_page, invalid_page_size · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found, resource_row_not_found · 409: run_queue_draft_driver_not_set, run_queue_draft_argument_selector_is_driver, run_queue_not_draft, run_queue_draft_changed, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 422: run_queue_draft_argument_selector_not_found, run_queue_draft_argument_entity_unsupported, invalid_resource_binding, run_inputs_missing · 503: auth_profile_unavailable, run_queue_unavailable

## list_queue_draft_driver_options
- does: List current driver candidates for an owned draft by query.queueId after its driver is set; use this instead of global resource lists for later draft driver choices.
- input: query { queueId: string; page?: string; size?: string; search?: string }
- returns: QueueOptionsResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request, invalid_page, invalid_page_size · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found, resource_row_not_found · 409: run_queue_draft_driver_not_set, run_queue_not_draft, run_queue_draft_changed, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 422: run_queue_draft_driver_entity_unsupported, invalid_resource_binding, run_inputs_missing · 503: auth_profile_unavailable, run_queue_unavailable

## list_queue_draft_device_options
- does: List current ready-and-free device candidates for an owned draft by query.queueId; use this for later draft worker choices instead of the global device list.
- input: query { queueId: string; page?: string; size?: string; search?: string }
- returns: QueueDeviceOptionsResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found · 409: run_queue_not_draft · 410: run_queue_deleted · 503: auth_profile_unavailable, run_queue_unavailable

## get_queue_draft
- does: Read one owned run queue draft.
- input: path { queue_id: string }
- returns: DraftSnapshotResponse
- flags: read-only
- refusals: 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found, preset_not_found, strategy_not_found, adapter_not_found, user_proxy_not_found, wifi_not_found, device_not_found · 409: run_queue_not_draft · 410: run_queue_deleted · 503: auth_profile_unavailable, run_queue_unavailable

## list_queue_draft_workers
- does: List workers assigned to an owned run queue draft.
- input: path { queue_id: string } · query { page?: string; size?: string }
- returns: DraftWorkersPageResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found · 409: run_queue_not_draft · 410: run_queue_deleted · 503: auth_profile_unavailable, run_queue_unavailable

## add_queue_draft_workers
- does: Add devices to an existing owned draft. `deviceIds` are UDID text from `list_devices` or list_queue_draft_device_options, not numeric row IDs. A new device must be owned, ready and available; an already-present member may return `alreadyPresent`. A lost response has no keyed receipt; read current workers before retrying.
- input: path { queue_id: string } · body DraftWorkersAddRequest { deviceIds: string[] }
- returns: DraftWorkersAddResponse
- flags: changes data
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found · 409: run_queue_worker_device_unavailable, run_queue_worker_in_use, run_queue_worker_cap_exceeded · 410: run_queue_deleted · 413: payload_too_large · 503: auth_profile_unavailable, run_queue_unavailable

## remove_queue_draft_workers
- does: Destructive: remove workers from an owned run queue draft.
- input: path { queue_id: string } · query { page?: string; size?: string } · body DraftWorkersDeleteRequest { workerIds: string[] }
- returns: DraftWorkersDeleteResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found · 409: run_queue_worker_assigned · 410: run_queue_deleted · 413: payload_too_large · 503: auth_profile_unavailable, run_queue_unavailable

## get_queue_draft_defaults
- does: Read argument defaults for an owned run queue draft.
- input: path { queue_id: string }
- returns: DraftDefaultsResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found, preset_not_found, strategy_not_found, adapter_not_found, user_proxy_not_found, wifi_not_found, device_not_found · 409: run_queue_not_draft, run_queue_draft_changed · 410: run_queue_deleted · 422: run_queue_draft_incomplete · 503: auth_profile_unavailable, run_queue_unavailable

## set_queue_draft_default
- does: Update argument defaults for an owned run queue draft.
- input: path { queue_id: string } · body DraftDefaultRequest {  }
- returns: DraftDefaultWriteResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found, adapter_not_found, user_proxy_not_found, resource_row_not_found · 409: run_queue_draft_changed, run_queue_not_draft, run_queue_draft_exclusive_default_forbidden, strategy_access_revoked, strategy_unavailable, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 413: payload_too_large · 422: run_queue_draft_incomplete, invalid_resource_binding, run_inputs_missing · 503: auth_profile_unavailable, run_queue_unavailable

## update_queue_draft_item_overrides
- does: Destructive: update adapter, proxy or non-driver argument overrides for scheduled draft items. This cannot change the driver value. `itemId` comes from `list_queue_draft_items`; each accepted write advances draft revision, so re-read before a retry after an uncertain result.
- input: body DraftItemsPutRequest { items: DraftItemPutRequest[]; queueId: string }
- returns: DraftItemWriteResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_item_not_found, run_queue_not_found, adapter_not_found, user_proxy_not_found, resource_row_not_found · 409: run_queue_item_entity_conflict, run_queue_item_entity_in_use, run_queue_draft_changed, run_queue_not_draft, strategy_access_revoked, strategy_unavailable, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 413: payload_too_large · 422: run_queue_draft_incomplete, invalid_resource_binding, run_inputs_missing · 503: auth_profile_unavailable, run_queue_unavailable

## add_queue_draft_items
- does: Add items to an owned run queue draft; a lost response has no keyed receipt.
- input: body DraftItemsAddRequest { items: DraftItemAddRequest[]; queueId: string }
- returns: DraftItemWriteResponse
- flags: changes data
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_item_not_found, run_queue_not_found, adapter_not_found, user_proxy_not_found, resource_row_not_found · 409: run_queue_item_entity_conflict, run_queue_item_entity_in_use, run_queue_draft_changed, run_queue_not_draft, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 413: payload_too_large · 422: run_queue_draft_incomplete, invalid_resource_binding, run_inputs_missing · 503: auth_profile_unavailable, run_queue_unavailable

## remove_queue_draft_items
- does: Destructive: remove items from an owned run queue draft.
- input: body DraftItemsDeleteRequest { items: DraftItemDeleteRequest[]; queueId: string }
- returns: DraftItemsDeleteResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_draft_item_not_found, run_queue_not_found · 409: run_queue_item_entity_conflict, run_queue_item_entity_in_use, run_queue_draft_changed, run_queue_not_draft · 410: run_queue_deleted · 413: payload_too_large · 422: run_queue_draft_incomplete · 503: auth_profile_unavailable, run_queue_unavailable

## list_queue_draft_items
- does: List items in an owned run queue draft.
- input: path { queue_id: string } · query { page?: integer; size?: integer }
- returns: DraftItemsPageResponse
- flags: read-only
- refusals: 400: run_queue_draft_invalid_request, invalid_page, invalid_page_size, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found, preset_not_found, strategy_not_found, adapter_not_found, user_proxy_not_found, wifi_not_found, device_not_found · 409: run_queue_not_draft, run_queue_draft_changed · 410: run_queue_deleted · 422: run_queue_draft_incomplete · 503: auth_profile_unavailable, run_queue_unavailable

## finalize_queue_draft
- does: Finalize a draft into a ready queue without starting work. `expectedRevision` must match the current draft revision from `get_queue_draft`; call `start_queue` only if execution is wanted. Before finalizing, inspect get_queue_draft, list_queue_draft_workers, list_queue_draft_items and get_queue_draft_defaults for the pin, driver, items, workers, required values and adapter. If the default llmAdapterId is null, pick an existing own adapter from list_adapters/list_adapter_options and call set_queue_draft_default with kind=llmAdapter and numeric llmAdapterId, then re-read the revision; adapter configuration is UI-only. One edit does not guarantee readiness.
- input: path { queue_id: string } · body RunQueueDraftRequest { expectedRevision: string }
- returns: DraftRunResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_draft_invalid_request, run_queue_limit_exceeded · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found, preset_not_found, strategy_not_found, adapter_not_found, user_proxy_not_found, wifi_not_found, device_not_found · 409: run_queue_draft_changed, run_queue_not_draft, run_queue_worker_in_use · 410: run_queue_deleted · 413: payload_too_large · 422: run_queue_draft_incomplete · 503: auth_profile_unavailable, run_queue_unavailable
