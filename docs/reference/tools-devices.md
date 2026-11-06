---
id: reference/tools-devices
generated: true
audience: both
---

# Tools — devices (9)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.

> A capitalised type name in an input (for example `QueueArgRequest`) is spelled out in request-shapes.md.


## list_devices
- does: List managed devices with pagination and projected status. `online` means a fresh snapshot; `ready` additionally requires projected readiness. Each row includes numeric `rowId` and UDID `deviceId`; check current availability before assigning work.
- input: query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string; search?: string; status?: string }
- returns: PaginatedResponse_DeviceListItemDto
- flags: read-only

## list_device_agent_updates
- does: Read only your live devices with advisory or required agent updates across the whole fleet, independent of inventory pages. complete=false means release evidence is unresolved; never prune dismissed notices from an incomplete or failed read. This does not install updates or change readiness.
- input: none
- returns: DeviceAgentUpdatesResponse
- flags: read-only

## get_device_summary
- does: Read your whole live-device fleet counts, including total and ready; independent of device list pages and filters. Stale, disconnected and missing evidence never counts as ready.
- input: none
- returns: DeviceSummaryDto
- flags: read-only

## device_detail
- does: Read one owned device's status and details.
- input: path { device_id: string }
- returns: DeviceDto
- flags: read-only

## get_device_screen
- does: Capture one NEW screenshot from your current phone session and return it as a real JPEG image block with safe text metadata: true pixel width/height, the frame's RFC3339 receivedAt backend receipt timestamp, captureCommandId and the canonical unsigned decimal string frameSeq. device_id is the exact UDID, not numeric rowId. Send no body or {}; every query parameter is rejected. This is a fresh capture, evidenced by the exact command/frame correlation, not the cached thumbnail or latest buffered frame, and no older image is substituted when the new one is missing or invalid. There is no Back action, and a capture may interleave with a running phone strategy. Freshness is the backend receipt time of the request-correlated frame, not a device capture latency or clock-synchronization claim. It charges the shared per-user device-command budget once per logical request and is never automatically retried.
- input: path { device_id: string } · body CaptureDeviceScreenRequest {  }
- returns: PhoneScreenResponse
- flags: read-only

## device_action
- does: Destructive: send exactly one closed safe action to your current phone session — tap_pct, long_press_pct, swipe_pct, scroll_pct, press_home, launch_app, close_app, accept_alert, dismiss_alert or type_text — and return the acknowledged receipt with an optional fresh post-action screen. Coordinates are screen fractions from 0 to 1, durations are milliseconds, text is at most 16384 UTF-8 bytes and bundle_id at most 256. delivery=acknowledged means delivery was accepted; effect=unverified means the UI may not have changed. An accepted action, including accept-alert, type-text or an app action, can have real consequences and is not guaranteed harmless or repairable. screen=null with screenError is still the acknowledged result, not a mutation failure: inspect fresh state with get_device_screen before issuing any new action. No Back action exists, generic /commands remains key-forbidden, and the action may interleave with a running strategy. It charges the shared per-user device-command budget once per logical request; never automatically repeat a mutation after a lost or uncertain reply.
- input: path { device_id: string } · body PhoneDeviceActionRequest { action: PhoneDeviceActionWire }
- returns: PhoneActionResponse
- flags: changes data · destructive: confirm with the user first

## list_device_apps
- does: Read a FRESH complete inventory of the apps installed on your phone's current session: name, bundleId, version and source (the phone's application type, including system and re-signed apps), sorted by bundleId. device_id is the exact UDID, not numeric rowId. Use it to learn the exact bundleId before device_action launch_app/close_app, and to see which variants of an app are installed. observedAt is the backend receipt time of this read, not a device capture time. A failed, offline or oversized inventory is a typed refusal, never an empty list. Send no query and no body. It charges the shared per-user device-command budget once per request; never cached.
- input: path { device_id: string }
- returns: DeviceAppsResponse
- flags: read-only

## set_device_app_choice
- does: Remember which approved bundle variant of a named app (for example TikTok) this phone should use, as desired state, when app_choice_required says several approved variants are installed. Send body {appName,bundleId}; bundleId must be an approved variant that a fresh inventory shows installed on this phone, otherwise app_choice_invalid or app_not_installed is returned (with appSelection candidates/suggestions as evidence only). It does not launch anything and gives no receipt that a later launch will use it. device_id is the exact UDID, not numeric rowId. It charges the shared per-user device-command budget once per request.
- input: path { device_id: string } · body SetDeviceAppChoiceRequest { appName: string; bundleId: string }
- returns: DeviceAppChoiceResponse
- flags: changes data
- refusals: 503: app_catalog_unavailable

## set_device_alias
- does: Set or clear the display alias of one of your phones, as owner-asserted desired state, not an agent observation. Send body {alias}: a string, trimmed and otherwise kept exactly (case, emoji and Unicode preserved), at most 64 Unicode characters after trimming, with no control characters; null or a whitespace-only string clears it. device_id is the exact UDID, not numeric rowId. It touches nothing on the phone, emits no event and sends nothing to the agent; Forget retains the alias. Success returns {deviceId, alias} with the canonical stored alias, null when cleared. If the reply is lost or uncertain, read device_detail before deciding: repeating the identical value is state-idempotent, but this call is never retried automatically. A missing, foreign or deleted device is one device_not_found. Body at most 1024 bytes, no query. Every outcome, including refusals, carries Cache-Control: no-store.
- input: path { device_id: string } · body SetDeviceAliasRequest { alias: string|null }
- returns: SetDeviceAliasResponse
- flags: changes data
- refusals: 503: devices_unavailable
