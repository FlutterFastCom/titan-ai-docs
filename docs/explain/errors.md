---
id: explain/errors
audience: both
intents: [error, failed, refused, not allowed, what does this error mean, why can't I]
tools: [wait_seconds]
error_codes: [access_not_granted, api_key_invalid, api_key_route_forbidden, auth_expired_token, mcp_rate_limited, strategy_authoring_disabled, run_inputs_missing, invalid_resource_binding, resource_row_not_found, resource_in_use, device_not_ready, device_trust_pending, usb_detached, screen_asleep, tunnel_failed, wda_not_ready, snapshot_stale, agent_disconnected, strategy_access_revoked, strategy_unavailable, strategy_not_found, bulk_conflict, resource_timeout, run_queue_draft_incomplete, run_queue_limit_exceeded, run_queue_worker_cap_exceeded, run_queue_worker_device_unavailable, user_proxy_dead, publish_post_invalid, payload_too_large, retry_budget_exhausted, phone_return_timeout]
sources:
  - be:src/adapters/http/run_execution/errors.rs
  - be:src/adapters/http/strategies/error_response.rs
  - be:src/adapters/http/resources/batch_errors.rs
  - be:src/adapters/http/mcp/error_guidance.rs
verified_be: 92a0a9c1
---
# Errors in plain words

A refusal answers `{status, error:{code, message, issues}}`. Read `issues` — it points at the exact field. Never show
the code to the user; say the plain meaning and do the fix. Which tool returns which code: `reference/error-codes.md`.

## Access and sign-in
| code | say | do |
|---|---|---|
| `access_not_granted` | "Your Titan access isn't active (or has expired)." | they must ask an administrator; check **Settings → User** for the date |
| `api_key_invalid`, `auth_expired_token` and other `auth_*` | "My connection key to your Titan account no longer works." | ask them to create a new key in **Settings → API keys** and update your connection |
| `api_key_route_forbidden` | — | this action isn't available through a key (often admin-only); don't retry |
| `strategy_authoring_disabled` | "Your account isn't allowed to build strategies yet." | an administrator must enable it; offer to run shared strategies meanwhile |
| `mcp_rate_limited` | (nothing unless long) | `wait_seconds` for `retryAfterSeconds` (split into ≤30 s waits), then continue |

## Running
| code | say | do |
|---|---|---|
| `run_inputs_missing`, `invalid_resource_binding` | "It still needs <the account / post / text>." | ask for it or pick it, then start again |
| `resource_row_not_found`, `resource_row_revision_conflict`, `resource_schema_version_mismatch` | "That <account> changed or was removed." | read it again and re-select |
| `resource_in_use` | "That account is busy in another run." | wait, or use another account |
| `device_not_ready`, `snapshot_stale`, `agent_disconnected` | "Your phone isn't ready right now." | `devices/index.md` |
| `device_trust_pending`, `usb_detached`, `screen_asleep` | "Unlock the phone / plug it in / tap Trust." | then check again |
| `tunnel_failed`, `wda_not_ready` | "The phone connection isn't ready." | `devices/tunnel-down.md`, `devices/automation-bridge.md` |
| `strategy_access_revoked`, `strategy_unavailable`, `strategy_not_found` | "That strategy isn't available to you anymore." | ask the person who shared it, or pick another |
| `publish_post_invalid` | "The post isn't ready to publish (photos missing or too many)." | `import/posts.md` — 1–9 items |
| `retry_budget_exhausted`, `phone_return_timeout` | "The step kept failing / the phone didn't come back in time." | `goals/fix-a-failed-run.md` |

## Batches across phones
| code | say | do |
|---|---|---|
| `run_queue_draft_incomplete` | "The batch isn't fully set up yet." | the refusal does not say what is missing: read the draft and its run settings, set the missing piece (often the AI service), read the draft again, then finalize |
| `run_queue_limit_exceeded`, `run_queue_worker_cap_exceeded` | "That's more than one batch can hold." | split it or use fewer phones |
| `run_queue_worker_device_unavailable` | "One of the phones isn't available." | drop it or fix it (`devices/index.md`) |
| other `run_queue_*` with "changed" or "conflict" | — | something changed meanwhile: read the current state, then retry once |

## Saving content
| code | say | do |
|---|---|---|
| `bulk_conflict` | "Some of these already exist or changed — nothing was saved yet." | remove duplicates / re-read, send again |
| `resource_timeout` | (nothing) | resend the SAME batch with the SAME key |
| `payload_too_large` | — | send fewer items per batch |
| `user_proxy_dead` | "That proxy doesn't answer." | ask them to check it or pick another |

Anything not listed: read `message` and `issues`, explain plainly, and never retry the same request unchanged more than
once. A code missing from `reference/error-codes.md` may mean these docs are out of date (see `CLAUDE.md`).
