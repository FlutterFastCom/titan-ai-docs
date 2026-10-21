---
id: goals/build-a-strategy
audience: author
intents: [create strategy, build automation, make my phone do, new strategy, improve strategy]
tools: [list_user_strategies, create_strategy]
error_codes: [strategy_authoring_disabled]
sources:
  - be:src/adapters/http/mcp/presentation.rs
verified_be: 92a0a9c1
---
# The user wants a new strategy

Do NOT start by explaining how strategies work. Find out what they want the phone to do, propose a design, build it.

## 1. Ask (one message, at most 3–4 questions, each with a default)
- "What should happen on the phone, step by step, in your words?" (the only required question)
- "Which app and which accounts?" — default: TikTok, their saved accounts.
- "Once, or repeated? How many times, how long between?" — default: once.
- "On one phone or several at the same time?" — default: one, then scale up.
Ask follow-ups only for what the chosen recipe needs (its "questions" list).

## 2. Propose
Pick the closest design from `recipes/index.md` and say it in 2–4 plain lines: "I'll set it up to: 1) log into the
account, 2) post the photos with your caption, 3) wait 20 minutes, repeat for the next account. Sound right?" If the
closest recipe is UNPROVEN, say so plainly: "This combination hasn't been tested end to end yet — I'll test it on one
phone first."

## 3. Build, test, report
`build/index.md` step by step, silently. Test on ONE phone. Report what worked in plain words, then offer the next
step ("Run it on all 4 phones?").

Already have a strategy that's close (`list_user_strategies`)? Prefer improving it (`build/7-improve-it.md`) over a new
one. Building refused with `strategy_authoring_disabled`: "Your account isn't allowed to build strategies — an
administrator has to turn that on. Meanwhile I can run strategies shared with you."
