---
id: goals/fix-a-failed-run
audience: both
intents: [run failed, why did it fail, error, stopped, it did not work, broken]
tools: [list_user_runs, get_user_run, list_user_run_steps, list_activity, get_user_message_screenshot, read_run_transcript, device_detail]
sources:
  - be:src/adapters/http/run_execution/errors.rs
  - be:src/adapters/http/mcp/presentation.rs
verified_be: 92a0a9c1
---
# A run failed — find out why and fix it

1. `list_user_runs` → the run → `get_user_run` and `list_user_run_steps`: which step failed, on which attempt.
2. Did it even start? A refusal before start (inputs, phone not ready, access) → `explain/errors.md` and the run's
   fix list in `build/5-run-it.md`. Phone problems → `devices/index.md`.
3. Failed during a step: `list_activity`, `get_user_message_screenshot` for what the phone showed; for the user's own
   strategies `read_run_transcript` for what the AI tried (`build/6-check-the-result.md`).
4. Tell the user in plain words: "It stopped while uploading the 2nd photo — TikTok showed a 'Save draft?' popup it
   didn't expect." Then ONE proposed fix:
   - phone state (locked, wrong app open, old popup) → "Unlock it and close TikTok, then I'll run it again";
   - missing or wrong content → fix the account/post, then rerun;
   - the strategy's instructions → if they own it: `build/7-improve-it.md`; if it is shared: "This needs a fix by the
     person who made it — I can tell you exactly what to report."
5. Never just retry the same run unchanged more than once. A step marked as posting must not be repeated before you
   checked whether it already posted.
