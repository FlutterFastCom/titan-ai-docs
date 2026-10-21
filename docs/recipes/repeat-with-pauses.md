---
id: recipes/repeat-with-pauses
audience: author
intents: [repeat, loop, every hour, pause between, many times, schedule]
tools: [save_definition, save_preset_plan]
sources:
  - be:src/application/strategies/definition.rs
  - be:src/application/run_execution/executor/progress.rs
  - be:src/application/strategy_presets/plan.rs
verified_be: 92a0a9c1
---
# Recipe — repeat with pauses   (mechanism confirmed)

- Repeat one step or a group: `defaultLoop:{times (1–1000), delayMs (fixed or [min,max] random range, up to 1 day),
  maxDurationMs (up to 7 days)}` on that step (`build/3-steps.md`).
- Repeat the whole strategy: the loop in the run settings plan (`get_preset_plan` → `save_preset_plan`).
- Across phones or accounts: a batch (`goals/run-at-scale.md`).
- A random pause range ("every 20–40 minutes") looks more natural than a fixed one — offer it.
- "Every day at 9:00" (calendar scheduling) is NOT confirmed to exist. Say so; offer a long loop with a delay instead,
  and explain it starts counting from when it begins.
- A repeated group counts as done if at least ONE pass succeeded — check every pass before reporting "all 10 posted".
