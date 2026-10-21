---
id: reference/tools-run-queue-live
generated: true
audience: both
---

# Tools — run-queue-live (12)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_queue_workers
- does: List workers in one owned live run queue.
- input: path { queue_id: string } · query { page?: string; size?: string; search?: string; statuses?: string; status?: string }
- returns: LiveWorkersPageResponse
- flags: read-only
- refusals: 400: run_queue_invalid_request · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found · 409: run_queue_is_draft · 410: run_queue_deleted · 503: auth_profile_unavailable, run_queue_unavailable

## list_queue_items
- does: List items in one owned live run queue.
- input: path { queue_id: string } · query { page?: string; size?: string; search?: string; statuses?: string; workerIds?: string }
- returns: LiveItemsPageResponse
- flags: read-only
- refusals: 400: run_queue_invalid_request · 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found · 409: run_queue_is_draft · 410: run_queue_deleted · 503: run_queue_unavailable

## start_queue
- does: Start a ready queue: committed status becomes active. Worker pickup timing is not guaranteed; inspect `list_queue_workers` and `list_queue_items` for progress (`steps.now` may be null).
- input: path { queue_id: string }
- returns: RunQueueLiveActionResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_invalid_request · 404: run_queue_not_found, adapter_not_found, user_proxy_not_found, wifi_not_found, device_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_busy, run_queue_worker_in_use, run_queue_driver_in_use · 422: publish_post_invalid · 503: run_queue_unavailable, auth_profile_unavailable, api_key_unavailable

## soft_pause_queue
- does: Pause queue scheduling without itself interrupting in-flight work. The queue becomes paused immediately while existing work may finish. `resume_queue` resumes scheduling.
- input: path { queue_id: string }
- returns: RunQueueLiveActionResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_invalid_request · 404: run_queue_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_busy

## hard_pause_queue
- does: Destructive: pause the queue and interrupt its current runs/items. The queue becomes paused. `resume_queue` permits future scheduling but cannot restore interrupted work.
- input: path { queue_id: string }
- returns: RunQueueLiveActionResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_invalid_request · 404: run_queue_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_busy

## resume_queue
- does: Resume a paused queue to active scheduling. Work interrupted by a prior hard pause is not restored.
- input: path { queue_id: string }
- returns: RunQueueLiveActionResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_invalid_request · 404: run_queue_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_busy

## stop_queue
- does: Destructive: terminally stop a ready, active or paused queue. Outstanding scheduled/running work is retired; succeeded history remains. The queue cannot be resumed.
- input: path { queue_id: string }
- returns: RunQueueLiveActionResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_invalid_request · 404: run_queue_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_busy

## cancel_queue_item
- does: Destructive: cancel one eligible live queue item (`list_queue_items` shows `canCancel`). Copy its `claimGeneration` into request `expectedClaimGeneration` and its `scheduleVersion` into `expectedScheduleVersion`; preserve an explicit `null` schedule version. If refused as not cancellable, re-read list_queue_items canCancel and lifecycle. A running item cannot be cancelled singly; stop_queue (terminal) and hard_pause_queue (pause with interruption) affect the whole queue including other items, so use them only when that wider effect is intended. Soft pause does not interrupt it; resume does not restore interrupted items.
- input: path { queue_id: string; item_id: string } · body LiveItemCancelRequest { expectedClaimGeneration: string; expectedScheduleVersion: string|null }
- returns: LiveItemCancelResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured · 403: access_not_granted · 404: run_queue_not_found, run_queue_item_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_item_not_cancellable, run_queue_item_changed, run_queue_busy · 410: run_queue_deleted · 413: payload_too_large · 503: run_queue_unavailable

## requeue_queue_item
- does: Requeue an eligible retired failed or cancelled live item (`list_queue_items` shows `canRequeue`), including a terminal linked run; use this broader recovery for cases outside the narrow proxy-unavailable/no-run path. Copy `claimGeneration` to request `expectedClaimGeneration` and `scheduleVersion` to `expectedScheduleVersion`, preserving explicit `null`. This may reopen a completed queue to ready or active and permit work to resume.
- input: path { queue_id: string; item_id: string } · body LiveItemRequeueRequest { expectedClaimGeneration: string; expectedScheduleVersion: string|null }
- returns: LiveItemRequeueResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 401: auth_missing_token, auth_malformed_token, auth_invalid_token, auth_expired_token, auth_not_configured, api_key_invalid · 403: access_not_granted, auth_profile_missing, api_key_route_forbidden · 404: run_queue_not_found, run_queue_item_not_found, user_proxy_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_item_not_requeueable, run_queue_item_changed, run_queue_worker_unavailable, run_queue_busy, resource_type_not_installed, resource_schema_version_mismatch, resource_row_revision_conflict · 410: run_queue_deleted · 413: payload_too_large · 422: user_proxy_dead, invalid_resource_binding, run_inputs_missing · 503: user_proxy_probe_unavailable, run_queue_unavailable

## pause_queue_worker
- does: Destructive: operator-pause one worker and interrupt its current work. `workerId` comes from `list_queue_workers`. The queue may be active or paused; this action leaves its status unchanged. `resume_queue_worker` permits future work for that worker.
- input: path { queue_id: string; worker_id: string }
- returns: RunQueueLiveWorkerActionResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_invalid_request · 404: run_queue_not_found, run_queue_worker_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_worker_status_conflict, run_queue_busy

## resume_queue_worker
- does: Clear one worker’s operator pause. `workerId` comes from `list_queue_workers`; the queue may be active or paused and its status is unchanged. New pickup still depends on queue state.
- input: path { queue_id: string; worker_id: string }
- returns: RunQueueLiveWorkerActionResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_invalid_request · 404: run_queue_not_found, run_queue_worker_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_worker_status_conflict, run_queue_busy

## bypass_queue_worker_cooldown
- does: Bypass cooldown for one worker in an owned live run queue.
- input: path { queue_id: string; worker_id: string }
- returns: RunQueueLiveWorkerBypassCooldownResponse
- flags: changes data · destructive: confirm with the user first
- refusals: 400: run_queue_invalid_request · 404: run_queue_not_found, run_queue_worker_not_found · 409: run_queue_is_draft, run_queue_status_conflict, run_queue_worker_cooldown_conflict, run_queue_busy
