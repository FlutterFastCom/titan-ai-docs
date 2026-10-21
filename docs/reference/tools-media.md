---
id: reference/tools-media
generated: true
audience: both
---

# Tools — media (3)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## get_media_library
- does: Read your agent-observed media metadata and saved pins, grouped by machineFingerprint. No file bytes. Copy machineFingerprint/contentHash from this result. connected, snapshotState and reachability describe current evidence; unknown is not missing. Read a baseline before select_media_folder/rescan_media; unchanged cached root/items cannot confirm either action. Seek a changed root or newly observed supported contentHash absent from baseline using wait_seconds, at most 12 readbacks/120 seconds, then report unconfirmed and stop polling.
- input: none
- returns: MediaLibraryResponse
- flags: read-only

## select_media_folder
- does: For unattended AI use always supply body.fallbackPath as an absolute folder on the paired agent PC; verify local existence and read permission first. Omission/null opens a native picker and can time out. This changes the selected folder, not file bytes. A 202 accepts the command only: a nonexistent/unreadable path can persist. Read get_media_library before the command, then use wait_seconds and seek the requested changed root or a newly observed supported contentHash absent from baseline on that machine. Stop after 12 readbacks or 120 seconds with an unconfirmed outcome if evidence is absent; do not blindly repeat the command.
- input: body SelectFolderRequest { fallbackPath?: string|null }
- returns: MediaCommandAcceptedResponse
- flags: changes data

## rescan_media
- does: Request a rescan of the paired agent's selected local folder. Omit body or send {}. A 202 is command acceptance, not completed indexing. Record a get_media_library baseline first; confirmation needs a newly observed supported contentHash absent from it, not old items. Poll with wait_seconds for at most 12 readbacks/120 seconds, then stop and report unconfirmed if no new evidence appears. Supported media only, at most 256 observed items and 100 MiB per file in the default agent; OS permission and inventory publication are required. No cloud file content or keyed replay.
- input: body RescanRequest {  }
- returns: MediaCommandAcceptedResponse
- flags: changes data
