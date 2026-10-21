---
id: build/4-make-it-live
audience: author
intents: [publish, release, make it live, share strategy, make public]
tools: [create_release, list_releases, get_release, set_reusable, add_shares, remove_shares, list_shares, publish_strategy, unpublish_strategy, withdraw_strategy_release, restore_strategy_release]
error_codes: [invalid_share_recipients, share_recipient_not_found]
sources:
  - be:src/adapters/http/mcp/presentation.rs
  - be:docs/business/features/shipped/F-STRATEGY-AUTHORING.md
  - be:src/adapters/http/strategies/error_response.rs
verified_be: 92a0a9c1
---
# Step 4 — make the saved steps live

Saving does not make anything runnable. A **release** seals one exact saved version.

1. Release every called sub-strategy first (each exact called version needs its own live release).
2. `create_release({path:{strategy_id}, body:{strategyVersionId:<versionId from the save>}})` → `releaseId`.
3. If other strategies should be able to call this one: `set_reusable({path:{strategy_id}, body:{releaseId, reusable:true}})`.

Release refused? A dependency is missing, archived, withdrawn, not reusable, or no longer yours. Fix that exact
dependency; never point at "the latest" instead.

## Share or publish (only when the user asks — confirm first)
- Share with people: `add_shares({path:{strategy_id}, body:{userIds:[...]}})` (up to 100). They can run every live
  release; they never see the steps or AI instructions. Undo: `remove_shares`.
- Public for everyone: `publish_strategy`. Undo: `unpublish_strategy`.
- Withdraw one bad release so nobody can start it anymore: `withdraw_strategy_release` (undo: `restore_strategy_release`).
Say it plainly first: "This makes it available to everyone on Titan. Go ahead?"
