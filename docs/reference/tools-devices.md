---
id: reference/tools-devices
generated: true
audience: both
---

# Tools — devices (6)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


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
