---
id: build/7-improve-it
audience: author
intents: [improve strategy, fix strategy, change a step, new version, iterate]
tools: [create_prompt_part_version, create_tool_set_version, create_prompt_part_preset_version, get_definition, save_definition, create_release, create_strategy_preset, put_preset_inputs, archive_strategy_preset]
sources:
  - be:src/adapters/http/mcp/presentation.rs
  - be:src/application/strategy_presets/save_plan.rs
  - be:src/application/strategy_presets/run_inputs.rs
verified_be: 92a0a9c1
---
# Step 7 — make it better

1. From step 6, name the one step that failed and why (the screen it was on, what it tried).
2. Change only that step's instructions or tools:
   `create_prompt_part_version` (and/or `create_tool_set_version`) → `create_prompt_part_preset_version` with the
   complete `{chatConfigAuthored, parts, toolSetVersionId}`.
3. `get_definition` → put the new composition version on that step → `save_definition` with the current
   `expectedVersionId` → `create_release` for the new version. If it is a sub-strategy, release it and then update and
   re-release every parent that calls it (pins never move by themselves).
4. Run settings are pinned to the old release. Create new run settings from the new release (`create_strategy_preset`),
   then optionally archive the old ones. `put_preset_inputs` only changes inputs, never the release.
5. Test once on one phone, then report: "Fixed the upload step — it now waits for the editor. It worked on iPhone A."

Change one thing per round. Two failed rounds on the same step: stop and tell the user what you have learned and what
you would try next.
