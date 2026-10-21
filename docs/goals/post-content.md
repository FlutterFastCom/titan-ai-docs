---
id: goals/post-content
audience: both
intents: [post photos, upload video, publish on tiktok, post my content, carousel]
tools: [list_user_strategies, get_media_library, list_resources, write_resources]
sources:
  - be:src/application/tools/photos_share.rs
  - be:src/application/resources/records.rs
verified_be: 92a0a9c1
---
# The user wants to post content

1. Ask: "Which photos or videos (a folder on your PC)? Which account? What caption? Public or private?" Defaults:
   their saved account, private for a first test.
2. Get the media into Titan and save the post: `import/posts.md`.
3. A posting strategy available? (`list_user_strategies`) → `goals/run-a-strategy.md`. None and the user can build →
   the proven design in `recipes/post-photos-logged-in.md`. None and they can't build → "You don't have a posting
   strategy yet — someone who builds strategies needs to share one with you."
4. Before a public post: confirm once. After: report what you verified ("The post is on the account's profile, 3
   photos, private").

What is proven today: a private carousel of 3 photos from an account already logged in on the phone (1–9 photos are
allowed, but other counts and public posts are not proven yet). Text placed on
photos and posting from many accounts in a row are not proven yet — say so before promising them.
