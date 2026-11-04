---
id: goals/share-a-strategy
audience: author
intents: [share strategy, give access, make public, stop sharing, who can use my strategy]
tools: [list_shares, add_shares, remove_shares, publish_strategy, unpublish_strategy, list_user_strategies]
error_codes: [invalid_share_recipients, share_recipient_not_found]
sources:
  - be:src/adapters/http/mcp/presentation.rs
  - be:src/adapters/http/strategies/error_response.rs
verified_be: 98be5af0
---
# Share a strategy

- Sharing lets named people RUN every live release of the strategy. They never see its steps or AI instructions.
- Public lets everyone on Titan run it.
- Sharing needs each person's Titan user ID (a long ID they can find in **Settings → User**, under **Technical
  details**). Ask: "Send me the User ID shown in their Settings → User." If the user only knows the person's Titan
  nickname, `list_shares({path:{strategy_id}, query:{lookup:"<nickname>"}})` finds matching people; say the
  nickname you found, confirm it is the right person, then share by that person's `userId`. An email address is not accepted.

Do: `list_shares` (who already has it) → confirm "Share '<strategy>' with these 2 people?" → `add_shares` (up to 100).
Stop sharing: `remove_shares`. Public: confirm "Everyone on Titan will be able to run it. OK?" → `publish_strategy`;
undo `unpublish_strategy`.

Using someone else's strategy: it appears automatically in `list_user_strategies` once shared or public — go to
`goals/run-a-strategy.md`.
