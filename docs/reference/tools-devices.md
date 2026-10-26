---
id: reference/tools-devices
generated: true
audience: both
---

# Tools — devices (5)

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

## recover_device_cleanup
- does: Destructive: explicitly remove TikTok and its local app data from one owned idle phone whose worker is blocked by a pending cleanup; device_id is the UDID, not numeric rowId. Body {"confirmation":"remove_tiktok"}. Refused while a run or chat is live or another recovery is in flight. Success (cleared or notNeeded) only after a fresh inventory shows TikTok absent and the hold clear is acknowledged; it does not resume a paused queue or retry the failed account. Never call it in the background or as automatic retry. After unavailable, uncertain or a timeout, read the device and worker state before any new attempt; do not blindly resend.
- input: path { device_id: string } · body CleanupRecoveryRequest { confirmation: CleanupRecoveryConfirmationRequest }
- returns: CleanupRecoveryResponse
- flags: changes data · destructive: confirm with the user first
