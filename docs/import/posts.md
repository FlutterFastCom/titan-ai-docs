---
id: import/posts
audience: both
intents: [import posts, upload photos, upload videos, media, carousel, prepare post]
tools: [get_media_library, select_media_folder, rescan_media, list_resource_types, get_resource_type, write_resources, wait_seconds]
sources:
  - be:src/application/resources/records.rs
  - be:src/adapters/http/auth_api_keys_catalog.rs
  - be:src/adapters/http/mcp/presentation.rs
verified_be: 92a0a9c1
---
# Import posts (photos / videos with their texts)

Media files never go to the cloud. They stay on the user's PC; Titan's PC Agent indexes a chosen folder and a post
points at a file by its machine and content hash.

## 1. Get the files into the PC Agent's media library
1. `get_media_library` — note what is already there (the baseline), grouped by `machineFingerprint`. Pick the user's
   connected PC and work only with that one machine from here on.
2. If the files are in another folder: `select_media_folder({body:{fallbackPath:"<absolute folder on the user's PC>"}})`.
   You cannot see the user's disk unless you run on that same PC — otherwise ask: "Which folder on your computer
   holds the photos? Please paste its full path." Then `rescan_media` to index it.
3. Poll `get_media_library` with `wait_seconds` until new files appear ON THAT SAME MACHINE, compared with a baseline
   read taken just before the rescan, and the library reports them as usable (at most 12 reads / 2 minutes). Files on
   another machine do not count. Nothing new →
   tell the user it is not confirmed and stop; do not repeat the command blindly. Limits: supported media only, up to
   256 files, 100 MiB each.

## 2. Build the post
Type `post` (exact schema from `get_resource_type`):
- `data.name` (required), `data.items` = ordered list, 1–9 items to be postable. Each item: `file` = the media
  reference copied from `get_media_library` (`kind`, `machineFingerprint`, `contentHash` — a file name alone is not
  enough), `text` (overlay text or null), `stickers` ([] if none), `filter` (null if none).
- Optional: `description` (caption), `music`, `location`, `privacy`, `coverIndex`, `allowComments`, `saveToDevice`,
  `aigcLabel`.
- One post = one carousel. Ask how files group: "One post with all 3 photos, or 3 separate posts?"

## 3. Write it
`write_resources` as in `import/import-a-file.md`. Report: "Saved the post 'Summer' with 3 photos and its caption."
