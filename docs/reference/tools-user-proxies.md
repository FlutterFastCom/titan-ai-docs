---
id: reference/tools-user-proxies
generated: true
audience: both
---

# Tools — user-proxies (5)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_user_proxies
- does: List the owner's LLM user proxies.
- input: none
- returns: UserProxiesResponse
- flags: read-only

## create_user_proxy
- does: Create one LLM user proxy owned by the key holder.
- input: body CreateUserProxyRequest { auth: UserProxyAuthRequest; enabled?: boolean|null; host: string; maxParallelRuns?: integer|null; name: string; scheme: "http"|"https" }
- returns: UserProxyResponse
- flags: changes data

## update_user_proxy
- does: Update one owned LLM user proxy.
- input: path { proxy_id: string } · body UpdateUserProxyRequest { auth?: null | UpdateUserProxyAuthRequest; enabled?: boolean|null; host?: string|null; maxParallelRuns?: integer|null; name?: string|null; scheme?: "http"|"https"|null }
- returns: UserProxyResponse
- flags: changes data · destructive: confirm with the user first

## delete_user_proxy
- does: Destructive: soft-delete one owned LLM user proxy, removing it from future selection.
- input: path { proxy_id: string }
- returns: (empty)
- flags: changes data · destructive: confirm with the user first

## probe_user_proxy
- does: Observe one owned proxy with the existing bounded fixed-target CONNECT/HEAD check; omit body or send {}. Returns alive/dead and checkedAt, not a permanent health guarantee; unavailable is not dead. One probe per owner/proxy, 32 globally, a five-second network-observation budget after descriptor resolution; authentication and database/descriptor work add latency. Respect retryAfterSeconds on user_proxy_probe_busy and wait_seconds before retrying. No proxy credentials or target body is returned.
- input: path { proxy_id: string } · body EmptyProbeBody {  }
- returns: UserProxyProbeResponse
- flags: read-only
