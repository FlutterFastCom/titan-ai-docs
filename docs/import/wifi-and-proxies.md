---
id: import/wifi-and-proxies
audience: both
intents: [import wifi, wifi networks, add proxies, proxy list]
tools: [create_wifi, list_wifis, create_user_proxy, list_user_proxies, probe_user_proxy, wait_seconds]
error_codes: [user_proxy_probe_busy]
sources:
  - be:src/domain/wifi.rs
  - be:src/adapters/http/auth_api_keys_catalog.rs
verified_be: 92a0a9c1
---
# Import Wi-Fi networks and proxies

Neither is a batch resource — create them one by one.

## Wi-Fi networks
`create_wifi({body:{name, ssid, security, password?, hidden?, autoJoin?}})` per network. Limits: name 200, SSID 64,
password 128 characters. `list_wifis` first to skip ones already saved. `get_wifi` returns the stored password — never
repeat it.

## Proxies (used for the AI service's traffic, not the phones)
`create_user_proxy({body:{name, scheme:"http"|"https", host, auth, enabled?, maxParallelRuns?}})` per proxy, then
`probe_user_proxy` to check it answers (alive/dead; "unavailable" is not dead). On `user_proxy_probe_busy` wait the
time it says with `wait_seconds`. Report: "Added 5 proxies — 4 answer, 1 doesn't (I left it saved but you may want to
check it)."
