---
id: goals/fix-a-failed-run
audience: both
intents: [run failed, why did it fail, error, stopped, it did not work, broken]
tools: [list_agent_diagnostics, list_device_apps, list_user_runs, get_user_run, list_user_run_steps, list_activity, get_user_message_screenshot, read_run_transcript, device_detail, get_device_screen, device_action]
---
# A run failed — find out why and fix it

1. `list_user_runs` → the run → `get_user_run` and `list_user_run_steps`: which step failed, on which attempt.
2. Did it even start? A refusal before start (inputs, phone not ready, access) → `explain/errors.md` and the run's
   fix list in `build/5-run-it.md`. Phone problems → `devices/index.md`.
3. Failed during a step: `list_activity`, `get_user_message_screenshot` for what the phone showed then (a stored
   picture — for what it shows NOW use `get_device_screen`); for the user's own
   strategies `read_run_transcript` for what the AI tried (`build/6-check-the-result.md`).
   A failed run's detail also carries a safe diagnosis — read it before guessing:
   - `failureClass`: `inputs` (a setting, account, AI service or proxy is wrong; `input` says which kind and field),
     `strategy` (the steps need a fix), `platform` (a Titan or phone-side fault), `unclear`. Its `code` can be
     `ai_action_limit` (the AI step ran out of actions) or `executor_retry_exhausted` (Titan gave up after repeated
     recovery tries; `executorRecovery` shows the count, at most 3).
   - `lastStepFailure`: where the last failed attempt stopped (a recognised reason and a closed failure label).
   - `lastCommandFailure`: the most recent failed phone command with its `cause` and `stage`
     (`explain/device-and-app-codes.md`). It is evidence only: a later action may have recovered from it, so do not call
     it the root cause. Phone causes: `devices/agent-diagnostics.md` (what the PC Agent itself reported), app causes:
     `devices/installed-apps.md`.
4. Tell the user in plain words: "It stopped while uploading the 2nd photo — TikTok showed a 'Save draft?' popup it
   didn't expect." Then ONE proposed fix:
   - phone state (locked, wrong app open, old popup) → "Unlock it and close TikTok, then I'll run it again". If the
     phone is unlocked and connected, you can look at its screen now and close the popup or app yourself, after the
     user says yes (`devices/control-the-phone.md`);
   - missing or wrong content → fix the account/post, then rerun;
   - the strategy's instructions → if they own it: `build/7-improve-it.md`; if it is shared: "This needs a fix by the
     person who made it — I can tell you exactly what to report."
   Nothing explains it, or the class is `platform` / `unclear`: say plainly that Titan has no recorded cause and that
   Titan support has to look; offer `devices/collect-logs.md`. Never guess a cause.
5. Never just retry the same run unchanged more than once. A step marked as posting must not be repeated before you
   checked whether it already posted.
