---
id: explain/codes-auth
audience: both
intents: [auth error, key invalid, not connected, connection key, token, profile missing, api key unavailable, sign in refused]
tools: [wait_seconds]
---
# Sign-in and connection-key codes

These refuse your own call before anything happens. Never show a code: say the plain sentence. "Transient" = wait
(`wait_seconds`, at most 30 s per call), read the state, retry once; still failing after a few minutes = a Titan-side fault.

| code | meaning | can you fix it | ask the user | only Titan |
|---|---|---|---|---|
| `auth_invalid_token` | the connection key is not recognised (wrong, revoked or deleted) | no | "My connection key to your Titan account no longer works. Please create a new one in **Settings → MCP API keys** and update my connection." | — |
| `auth_malformed_token` | the key could not be read as a key | no | same as above | — |
| `auth_missing_token` | no key was sent with the call | no | "I'm not connected to your Titan account. Please add your key in my connection settings." | — |
| `auth_not_configured` | Titan could not check the key because its sign-in check was unavailable | transient: wait, retry once | — | if it persists |
| `auth_profile_missing` | the key is valid but the account behind it has no Titan profile | no | "Your Titan account isn't fully set up. Please ask your Titan administrator to check it." | an administrator or Titan fixes the profile |
| `auth_profile_unavailable` | Titan could not load the account profile just now | transient | — | if it persists |
| `api_key_unavailable` | Titan could not check or manage keys just now | transient | — | if it persists |

Other sign-in codes (`access_not_granted`, `auth_expired_token`, `api_key_invalid`): `explain/errors.md`.
