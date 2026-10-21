---
id: reference/tools-wifis
generated: true
audience: both
---

# Tools — wifis (5)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_wifis
- does: List the owner's saved WiFi profiles.
- input: query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string }
- returns: PaginatedResponse_WifiSummaryResponse
- flags: read-only

## create_wifi
- does: Create one saved WiFi profile owned by the key holder.
- input: body CreateWifiRequest { autoJoin?: boolean; hidden?: boolean; name: string; password?: string; security: WifiSecuritySchema; ssid: string }
- returns: WifiDetailResponse
- flags: changes data

## get_wifi
- does: Read one owned WiFi profile including its stored password. Keep the returned credential private.
- input: path { wifi_id: string }
- returns: WifiDetailResponse
- flags: read-only · returns a stored credential: never repeat it to anyone

## update_wifi
- does: Update one owned WiFi profile.
- input: path { wifi_id: string } · body UpdateWifiRequest { autoJoin?: boolean|null; hidden?: boolean|null; name?: string|null; password?: string|null; security?: null | WifiSecuritySchema; ssid?: string|null }
- returns: WifiDetailResponse
- flags: changes data · destructive: confirm with the user first

## delete_wifi
- does: Destructive: permanently delete one owned WiFi profile.
- input: path { wifi_id: string }
- returns: (empty)
- flags: changes data · destructive: confirm with the user first
