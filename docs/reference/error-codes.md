---
id: reference/error-codes
generated: true
audience: both
---

# Error codes

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


Plain meanings and fixes live in docs/explain/errors.md. This table only says which tool returns which code.


| code | returned by |
|---|---|
| access_not_granted | delete_queue_drafts (403), create_empty_queue_draft (403), get_queue_draft_create_receipt (403), edit_queue_draft (403), get_queue_draft_edit_receipt (403), clone_queue_as_draft (403), get_queue_draft_clone_receipt (403), list_queue_draft_argument_options (403), list_queue_draft_driver_options (403), list_queue_draft_device_options (403), get_queue_draft (403), list_queue_draft_workers (403), … (25) |
| adapter_not_found | create_empty_queue_draft (404), edit_queue_draft (404), clone_queue_as_draft (404), get_queue_draft (404), get_queue_draft_defaults (404), set_queue_draft_default (404), update_queue_draft_item_overrides (404), add_queue_draft_items (404), list_queue_draft_items (404), finalize_queue_draft (404), start_queue (404) |
| agent_disconnected |  |
| api_key_invalid | requeue_queue_item (401) |
| api_key_route_forbidden | requeue_queue_item (403) |
| api_key_unavailable | start_queue (503) |
| auth_expired_token | delete_queue_drafts (401), create_empty_queue_draft (401), get_queue_draft_create_receipt (401), edit_queue_draft (401), get_queue_draft_edit_receipt (401), clone_queue_as_draft (401), get_queue_draft_clone_receipt (401), list_queue_draft_argument_options (401), list_queue_draft_driver_options (401), list_queue_draft_device_options (401), get_queue_draft (401), list_queue_draft_workers (401), … (25) |
| auth_invalid |  |
| auth_invalid_token | delete_queue_drafts (401), create_empty_queue_draft (401), get_queue_draft_create_receipt (401), edit_queue_draft (401), get_queue_draft_edit_receipt (401), clone_queue_as_draft (401), get_queue_draft_clone_receipt (401), list_queue_draft_argument_options (401), list_queue_draft_driver_options (401), list_queue_draft_device_options (401), get_queue_draft (401), list_queue_draft_workers (401), … (25) |
| auth_malformed_token | delete_queue_drafts (401), create_empty_queue_draft (401), get_queue_draft_create_receipt (401), edit_queue_draft (401), get_queue_draft_edit_receipt (401), clone_queue_as_draft (401), get_queue_draft_clone_receipt (401), list_queue_draft_argument_options (401), list_queue_draft_driver_options (401), list_queue_draft_device_options (401), get_queue_draft (401), list_queue_draft_workers (401), … (25) |
| auth_missing |  |
| auth_missing_token | delete_queue_drafts (401), create_empty_queue_draft (401), get_queue_draft_create_receipt (401), edit_queue_draft (401), get_queue_draft_edit_receipt (401), clone_queue_as_draft (401), get_queue_draft_clone_receipt (401), list_queue_draft_argument_options (401), list_queue_draft_driver_options (401), list_queue_draft_device_options (401), get_queue_draft (401), list_queue_draft_workers (401), … (25) |
| auth_not_configured | delete_queue_drafts (401), create_empty_queue_draft (401), get_queue_draft_create_receipt (401), edit_queue_draft (401), get_queue_draft_edit_receipt (401), clone_queue_as_draft (401), get_queue_draft_clone_receipt (401), list_queue_draft_argument_options (401), list_queue_draft_driver_options (401), list_queue_draft_device_options (401), get_queue_draft (401), list_queue_draft_workers (401), … (25) |
| auth_profile_missing | requeue_queue_item (403) |
| auth_profile_unavailable | delete_queue_drafts (503), create_empty_queue_draft (503), get_queue_draft_create_receipt (503), edit_queue_draft (503), get_queue_draft_edit_receipt (503), clone_queue_as_draft (503), get_queue_draft_clone_receipt (503), list_queue_draft_argument_options (503), list_queue_draft_driver_options (503), list_queue_draft_device_options (503), get_queue_draft (503), list_queue_draft_workers (503), … (23) |
| automatic_recovery |  |
| chat_conflict |  |
| cleanup_pending |  |
| device_not_found | create_empty_queue_draft (404), edit_queue_draft (404), clone_queue_as_draft (404), get_queue_draft (404), get_queue_draft_defaults (404), list_queue_draft_items (404), finalize_queue_draft (404), start_queue (404) |
| device_trust_pending |  |
| host_dep_missing |  |
| invalid_attempt_evidence |  |
| invalid_leaf |  |
| invalid_occurrence |  |
| invalid_outcome |  |
| invalid_owner |  |
| invalid_page | list_queue_draft_argument_options (400), list_queue_draft_driver_options (400), list_queue_draft_items (400) |
| invalid_page_size | list_queue_draft_argument_options (400), list_queue_draft_driver_options (400), list_queue_draft_items (400) |
| invalid_resource_binding | create_empty_queue_draft (422), edit_queue_draft (422), clone_queue_as_draft (422), list_queue_draft_argument_options (422), list_queue_draft_driver_options (422), set_queue_draft_default (422), update_queue_draft_item_overrides (422), add_queue_draft_items (422), requeue_queue_item (422) |
| irreversible_policy |  |
| observability_lost |  |
| payload_too_large | create_empty_queue_draft (413), edit_queue_draft (413), clone_queue_as_draft (413), add_queue_draft_workers (413), remove_queue_draft_workers (413), set_queue_draft_default (413), update_queue_draft_item_overrides (413), add_queue_draft_items (413), remove_queue_draft_items (413), finalize_queue_draft (413), cancel_queue_item (413), requeue_queue_item (413) |
| phone_return_timeout |  |
| presence_source_down |  |
| presence_uncertain |  |
| preset_not_found | create_empty_queue_draft (404), edit_queue_draft (404), clone_queue_as_draft (404), get_queue_draft (404), get_queue_draft_defaults (404), list_queue_draft_items (404), finalize_queue_draft (404) |
| publish_post_invalid | create_empty_queue_draft (422), edit_queue_draft (422), clone_queue_as_draft (422), start_queue (422) |
| quarantined |  |
| reason_unknown |  |
| recovery_in_progress |  |
| replay_conflict |  |
| resource_row_not_found | create_empty_queue_draft (404), edit_queue_draft (404), clone_queue_as_draft (404), list_queue_draft_argument_options (404), list_queue_draft_driver_options (404), set_queue_draft_default (404), update_queue_draft_item_overrides (404), add_queue_draft_items (404) |
| resource_row_revision_conflict | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409), list_queue_draft_argument_options (409), list_queue_draft_driver_options (409), set_queue_draft_default (409), update_queue_draft_item_overrides (409), add_queue_draft_items (409), requeue_queue_item (409) |
| resource_schema_version_mismatch | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409), list_queue_draft_argument_options (409), list_queue_draft_driver_options (409), set_queue_draft_default (409), update_queue_draft_item_overrides (409), add_queue_draft_items (409), requeue_queue_item (409) |
| resource_type_not_installed | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409), list_queue_draft_argument_options (409), list_queue_draft_driver_options (409), set_queue_draft_default (409), update_queue_draft_item_overrides (409), add_queue_draft_items (409), requeue_queue_item (409) |
| retry_budget_exhausted |  |
| run_inputs_missing | create_empty_queue_draft (422), edit_queue_draft (422), clone_queue_as_draft (422), list_queue_draft_argument_options (422), list_queue_draft_driver_options (422), set_queue_draft_default (422), update_queue_draft_item_overrides (422), add_queue_draft_items (422), requeue_queue_item (422) |
| run_queue_busy | start_queue (409), soft_pause_queue (409), hard_pause_queue (409), resume_queue (409), stop_queue (409), cancel_queue_item (409), requeue_queue_item (409), pause_queue_worker (409), resume_queue_worker (409), bypass_queue_worker_cooldown (409) |
| run_queue_clone_source_changed | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409) |
| run_queue_clone_source_not_cloneable | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409) |
| run_queue_clone_unsupported | create_empty_queue_draft (422), edit_queue_draft (422), clone_queue_as_draft (422) |
| run_queue_deleted | create_empty_queue_draft (410), edit_queue_draft (410), clone_queue_as_draft (410), list_queue_draft_argument_options (410), list_queue_draft_driver_options (410), list_queue_draft_device_options (410), get_queue_draft (410), list_queue_draft_workers (410), add_queue_draft_workers (410), remove_queue_draft_workers (410), get_queue_draft_defaults (410), set_queue_draft_default (410), … (21) |
| run_queue_draft_argument_entity_unsupported | list_queue_draft_argument_options (422) |
| run_queue_draft_argument_selector_is_driver | list_queue_draft_argument_options (409) |
| run_queue_draft_argument_selector_not_found | list_queue_draft_argument_options (422) |
| run_queue_draft_changed | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409), list_queue_draft_argument_options (409), list_queue_draft_driver_options (409), get_queue_draft_defaults (409), set_queue_draft_default (409), update_queue_draft_item_overrides (409), add_queue_draft_items (409), remove_queue_draft_items (409), list_queue_draft_items (409), finalize_queue_draft (409) |
| run_queue_draft_driver_entity_unsupported | list_queue_draft_driver_options (422) |
| run_queue_draft_driver_not_set | list_queue_draft_argument_options (409), list_queue_draft_driver_options (409) |
| run_queue_draft_exclusive_default_forbidden | set_queue_draft_default (409) |
| run_queue_draft_incomplete | create_empty_queue_draft (422), edit_queue_draft (422), clone_queue_as_draft (422), get_queue_draft_defaults (422), set_queue_draft_default (422), update_queue_draft_item_overrides (422), add_queue_draft_items (422), remove_queue_draft_items (422), list_queue_draft_items (422), finalize_queue_draft (422) |
| run_queue_draft_invalid_request | delete_queue_drafts (400), create_empty_queue_draft (400), get_queue_draft_create_receipt (400), edit_queue_draft (400), get_queue_draft_edit_receipt (400), clone_queue_as_draft (400), get_queue_draft_clone_receipt (400), list_queue_draft_argument_options (400), list_queue_draft_driver_options (400), list_queue_draft_device_options (400), list_queue_draft_workers (400), add_queue_draft_workers (400), … (20) |
| run_queue_draft_item_not_found | update_queue_draft_item_overrides (404), add_queue_draft_items (404), remove_queue_draft_items (404) |
| run_queue_draft_operation_conflict | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409) |
| run_queue_draft_operation_not_found | create_empty_queue_draft (404), get_queue_draft_create_receipt (404), edit_queue_draft (404), get_queue_draft_edit_receipt (404), clone_queue_as_draft (404), get_queue_draft_clone_receipt (404) |
| run_queue_draft_reset_required | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409) |
| run_queue_driver_in_use | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409), start_queue (409) |
| run_queue_invalid_request | list_queue_workers (400), list_queue_items (400), start_queue (400), soft_pause_queue (400), hard_pause_queue (400), resume_queue (400), stop_queue (400), pause_queue_worker (400), resume_queue_worker (400), bypass_queue_worker_cooldown (400) |
| run_queue_is_draft | list_queue_workers (409), list_queue_items (409), start_queue (409), soft_pause_queue (409), hard_pause_queue (409), resume_queue (409), stop_queue (409), cancel_queue_item (409), requeue_queue_item (409), pause_queue_worker (409), resume_queue_worker (409), bypass_queue_worker_cooldown (409) |
| run_queue_item_changed | cancel_queue_item (409), requeue_queue_item (409) |
| run_queue_item_entity_conflict | update_queue_draft_item_overrides (409), add_queue_draft_items (409), remove_queue_draft_items (409) |
| run_queue_item_entity_in_use | update_queue_draft_item_overrides (409), add_queue_draft_items (409), remove_queue_draft_items (409) |
| run_queue_item_not_cancellable | cancel_queue_item (409) |
| run_queue_item_not_found | cancel_queue_item (404), requeue_queue_item (404) |
| run_queue_item_not_requeueable | requeue_queue_item (409) |
| run_queue_limit_exceeded | delete_queue_drafts (400), create_empty_queue_draft (400), edit_queue_draft (400), clone_queue_as_draft (400), add_queue_draft_workers (400), remove_queue_draft_workers (400), get_queue_draft_defaults (400), set_queue_draft_default (400), update_queue_draft_item_overrides (400), add_queue_draft_items (400), remove_queue_draft_items (400), list_queue_draft_items (400), … (13) |
| run_queue_not_draft | create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409), list_queue_draft_argument_options (409), list_queue_draft_driver_options (409), list_queue_draft_device_options (409), get_queue_draft (409), list_queue_draft_workers (409), get_queue_draft_defaults (409), set_queue_draft_default (409), update_queue_draft_item_overrides (409), add_queue_draft_items (409), … (15) |
| run_queue_not_found | create_empty_queue_draft (404), get_queue_draft_create_receipt (404), edit_queue_draft (404), get_queue_draft_edit_receipt (404), clone_queue_as_draft (404), get_queue_draft_clone_receipt (404), list_queue_draft_argument_options (404), list_queue_draft_driver_options (404), list_queue_draft_device_options (404), get_queue_draft (404), list_queue_draft_workers (404), add_queue_draft_workers (404), … (32) |
| run_queue_status_conflict | start_queue (409), soft_pause_queue (409), hard_pause_queue (409), resume_queue (409), stop_queue (409), cancel_queue_item (409), requeue_queue_item (409), pause_queue_worker (409), resume_queue_worker (409), bypass_queue_worker_cooldown (409) |
| run_queue_unavailable | delete_queue_drafts (503), create_empty_queue_draft (503), get_queue_draft_create_receipt (503), edit_queue_draft (503), get_queue_draft_edit_receipt (503), clone_queue_as_draft (503), get_queue_draft_clone_receipt (503), list_queue_draft_argument_options (503), list_queue_draft_driver_options (503), list_queue_draft_device_options (503), get_queue_draft (503), list_queue_draft_workers (503), … (26) |
| run_queue_worker_assigned | remove_queue_draft_workers (409) |
| run_queue_worker_cap_exceeded | add_queue_draft_workers (409) |
| run_queue_worker_cooldown_conflict | bypass_queue_worker_cooldown (409) |
| run_queue_worker_device_unavailable | add_queue_draft_workers (409) |
| run_queue_worker_in_use | create_queue_draft (409), create_empty_queue_draft (409), edit_queue_draft (409), clone_queue_as_draft (409), add_queue_draft_workers (409), finalize_queue_draft (409), start_queue (409) |
| run_queue_worker_not_found | pause_queue_worker (404), resume_queue_worker (404), bypass_queue_worker_cooldown (404) |
| run_queue_worker_status_conflict | pause_queue_worker (409), resume_queue_worker (409) |
| run_queue_worker_unavailable | requeue_queue_item (409) |
| screen_asleep |  |
| snapshot_missing |  |
| snapshot_stale |  |
| strategy_access_revoked | set_queue_draft_default (409), update_queue_draft_item_overrides (409) |
| strategy_not_found | create_empty_queue_draft (404), edit_queue_draft (404), clone_queue_as_draft (404), get_queue_draft (404), get_queue_draft_defaults (404), list_queue_draft_items (404), finalize_queue_draft (404) |
| strategy_unavailable | set_queue_draft_default (409), update_queue_draft_item_overrides (409) |
| trust_required |  |
| tunnel_failed |  |
| unknown |  |
| usb_detached |  |
| user_proxy_dead | requeue_queue_item (422) |
| user_proxy_not_found | create_empty_queue_draft (404), edit_queue_draft (404), clone_queue_as_draft (404), get_queue_draft (404), get_queue_draft_defaults (404), set_queue_draft_default (404), update_queue_draft_item_overrides (404), add_queue_draft_items (404), list_queue_draft_items (404), finalize_queue_draft (404), start_queue (404), requeue_queue_item (404) |
| user_proxy_probe_unavailable | requeue_queue_item (503) |
| wda_assignment_missing |  |
| wda_not_ready |  |
| wifi_not_found | create_empty_queue_draft (404), edit_queue_draft (404), clone_queue_as_draft (404), get_queue_draft (404), get_queue_draft_defaults (404), list_queue_draft_items (404), finalize_queue_draft (404), start_queue (404) |
