---
id: reference/tools-device-media
generated: true
audience: both
---

# Tools — device-media (2)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## screen_thumb_jpeg
- does: Read the owned device's buffered JPEG thumbnail as image content; device_id is its UDID, not numeric rowId. Prefer this over screen_latest_jpeg for polling. Optional query.w accepts 1..320; cached thumbnail size follows broker configuration. The read may warm the existing agent thumbnail stream. frameAgeMs/frameState describe the returned frame; age measures time since backend receipt, not device capture time. screen_frame_absent means no buffered frame, not a failed JSON parser. Read device_detail/list_devices and wait_seconds before polling again.
- input: path { device_id: string } · query { w?: integer }
- returns: image
- flags: read-only

## screen_latest_jpeg
- does: Read the owned device's latest buffered full JPEG as image content with frameAgeMs/frameState; this does not capture a fresh screenshot. Prefer screen_thumb_jpeg for polling. A stalled frame is historical evidence. screen_frame_absent is a typed no-frame refusal. The encoded result is bounded to 8 MiB; use the thumbnail if it is too large.
- input: path { device_id: string }
- returns: image
- flags: read-only
