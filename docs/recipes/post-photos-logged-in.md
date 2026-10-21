---
id: recipes/post-photos-logged-in
audience: author
intents: [post photos, tiktok carousel, upload photos tiktok, publish post]
tools: [get_media_library, write_resources, create_strategy, save_definition, create_release]
sources:
  - conductor:missions/2026-10-18-tiktok-upload-native-photos-step-gets-no-ordered/QUEUE.md
  - be:src/application/tools/photos_share.rs
  - be:src/domain/tools/registry.rs
verified_be: 92a0a9c1
verified_conductor: 732df11a
---
# Recipe — post a photo carousel from an account already logged in   (PROVEN)

Proven shape: 3 photos, private audience, no text on the photos, on a phone where TikTok was already logged into the
intended account. Other counts (1–9) are allowed by the tools; public audience and text-on-photo are not proven.

## Parts (three sub-strategies, called in this order)
1. **Prepare** — AI steps: set the profile description (if wanted) and prepare the post's media on the phone (the
   media-preparation tool). Start from a known screen: close TikTok first — a leftover post editor caused a
   "Save draft / Discard" loop.
2. **Hand the photos to TikTok** — one AI step using `share_photos_to_tiktok`: opens Photos, selects the NEWEST N
   photos (by position, not by identity — so step 1 must have just added exactly these), shares them to TikTok. It
   ends at tapping TikTok in the share sheet; it does not prove TikTok's editor opened.
3. **Edit and post** — AI steps: choose **Photo** on TikTok's handoff sheet (a separate step — without it runs
   failed), skip overlays when there is no text, pick music, Next, fill the caption, set the audience, Post. Mark the
   Post step `irreversible`. Success = the post is visible on the account's own profile (for a private post, under the
   lock tab — not on the public grid).

## Inputs the user chooses
Phone · which saved post (photos in order) · profile description (optional) · caption · audience · music.
Give caption and audience their own inputs — a saved post does NOT fill them automatically (a real run failed on this).

## Ask
"Which phone, and is TikTok already logged into the right account on it?" · "Which photos, in what order?" · "Caption?"
· "Public or private?" · "Any music?"
