---
id: recipes/index
audience: author
intents: [which strategy, best strategy, recommend, what design, propose strategy]
tools: [list_tools]
sources:
  - be:src/application/strategies/definition.rs
verified_be: 92a0a9c1
---
# Strategy recipes — pick the closest, never invent

PROVEN = it ran green on a real iPhone at least once in the recorded shape. It does NOT mean reliable on every phone.
UNPROVEN = the design exists but no complete green run was recorded: tell the user, then test on one phone first.

| the user wants | recipe | status |
|---|---|---|
| post photos from the account already logged in on the phone | `recipes/post-photos-logged-in.md` | PROVEN for 3 photos, private, no text on photos (1–9 allowed; other shapes not yet proven) |
| log into a TikTok account (with the email login code) | `recipes/tiktok-login.md` | PROVEN |
| reset the phone's network settings and wait for it to come back | `recipes/network-reset.md` | PROVEN |
| connect the phone's VPN to a location | `recipes/vpn-connect.md` | PROVEN (ExpressVPN only) |
| remove TikTok and install it fresh | `recipes/reinstall-tiktok.md` | parts PROVEN separately |
| the full cycle: reset → VPN → install → login → post → clean up | `recipes/full-cycle.md` | UNPROVEN end to end |
| post from many accounts, one after another | `recipes/many-accounts.md` | UNPROVEN |
| repeat with pauses | `recipes/repeat-with-pauses.md` | mechanism confirmed |

How recipes are built: small sub-strategies (one job each, each with its own AI steps and tools), released, then
called in order by a parent strategy with `call` steps. Build and release the children first. Confirm each phone tool
exists with `list_tools` before using it.

Something the user wants that no recipe covers (text overlays on photos, Instagram, another VPN app, a calendar
schedule): say it is not proven or not available, offer the closest recipe, and test on one phone before anything else.
