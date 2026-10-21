---
id: reference/tools-devices
generated: true
audience: both
---

# Tools — devices (4)

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
