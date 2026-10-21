---
id: build/index
audience: author
intents: [build a strategy, create a strategy, make an automation, how do I build]
tools: [create_strategy, save_definition, create_release]
sources:
  - be:src/adapters/http/mcp/presentation.rs
verified_be: 92a0a9c1
---
# Building a strategy — the order (for you; the user never sees these steps)

Only someone allowed to build strategies can do this. If `create_strategy` answers `strategy_authoring_disabled`, stop
and tell the user: "Your account isn't allowed to build strategies yet — an administrator has to turn that on."

Before any of this, run the interview in `goals/build-a-strategy.md` and pick a design from `recipes/index.md`.

| step | file | result you keep |
|---|---|---|
| 1 | `build/1-content.md` | the user's accounts, descriptions, posts saved as resources (or import them: `import/import-a-file.md`) |
| 2 | `build/2-ai-steps.md` | for each AI step: a tool set version, prompt part versions, one prompt composition version |
| 3 | `build/3-steps.md` | the strategy and its saved steps (a definition version) |
| 4 | `build/4-make-it-live.md` | a release (and shares, if wanted) |
| 5 | `build/5-run-it.md` | run settings, a first test run on one phone |
| 6 | `build/6-check-the-result.md` | what happened, from the run's steps, screenshots and AI transcript |
| 7 | `build/7-improve-it.md` | a better version, released, with new run settings |

Rules for the whole flow:
- Copy every ID from a tool response; never build one from a name.
- Pins never move by themselves: an AI step uses an exact composition version, a sub-strategy call uses an exact
  version, run settings use an exact release.
- Build sub-strategies first (they must be released before a parent can be released), then the parent.
- Test on ONE phone before any batch.
- Tell the user progress in plain words: "Steps saved. Making it live now." — never the IDs.
