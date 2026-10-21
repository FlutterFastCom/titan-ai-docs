---
id: recipes/full-cycle
audience: author
intents: [full cycle, reset vpn login post cleanup, complete automation, fresh device each time]
tools: [create_strategy, save_definition, create_release]
sources:
  - conductor:missions/2026-10-18-tiktok-upload-native-photos-step-gets-no-ordered/QUEUE.md
verified_conductor: 732df11a
---
# Recipe — the full cycle   (UNPROVEN end to end)

A parent strategy that calls, in order: clean up → network reset → VPN → (optional leak check) → install TikTok →
login → post → clean up. Each part is its own recipe; most are proven alone, the whole chain has no recorded green run.

Tell the user before building: "Each piece has worked on its own, but the full chain hasn't been run end to end yet.
I'll build it and test it once on one phone, watching each step."

Build: release every child first; the parent's `call` steps pin each child's exact version. Inputs: phone, VPN
location, account (with login-code setup), post, caption, audience. The leak check only measures; it does not stop the
run or guarantee anonymity — don't present it as protection.

## Ask
"Is the network reset needed every time? Must TikTok be removed at the end? Which VPN location?"
