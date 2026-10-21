---
id: build/6-check-the-result
audience: both
intents: [did it work, what happened, why did it fail, show the run, transcript]
tools: [get_user_run, list_activity, list_user_run_passes, list_user_run_steps, read_run_transcript, get_user_message_screenshot, wait_seconds]
error_codes: [transcript_changed]
sources:
  - be:src/adapters/http/mcp/presentation.rs
  - be:src/application/run_execution/executor/progress.rs
verified_be: 92a0a9c1
---
# Step 6 — watch the run and read what happened

- Progress: `get_user_run` every few seconds with `wait_seconds` (1–30) in between. Tell the user milestones, not every
  poll: "Logging in… done. Uploading the photos now."
- Which step, which attempt: `list_user_run_passes`, `list_user_run_steps`; the activity feed: `list_activity`.
- What the AI saw and did (only for strategies the user owns): `read_run_transcript` with the step attempt's chat ID,
  page with `nextCursor` until null; on `transcript_changed` start again without a cursor. A screenshot of a moment:
  `get_user_message_screenshot`.
- A green run is not proof every repetition worked (a loop is done when one pass succeeded) — check the passes.
- Never repeat passwords, codes or account data you find in a transcript.

When reporting a failure, say: which step, what the phone showed, the likely cause, and the one fix you propose.
Then follow `build/7-improve-it.md` (builders) or `goals/fix-a-failed-run.md` (everyone).
