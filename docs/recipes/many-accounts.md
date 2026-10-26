---
id: recipes/many-accounts
audience: author
intents: [many accounts, rotate accounts, each account posts, campaign, all my accounts]
tools: [preview_queue, create_and_start_queue]
sources:
  - be:src/application/run_execution/queue/generator.rs
  - be:src/application/run_execution/queue/contracts.rs
verified_be: 92a0a9c1
---
# Recipe — post from many accounts   (PROVEN per run: the full cycle ran green on 2 different accounts, 2026-10-24; a long batch has no recorded green yet)

Shape: a strategy whose account (and post) is an input, run as a batch where each run gets the next account
(`goals/run-at-scale.md`: `driver` = one run per account; `cycle` = rotate a list). On one phone, every run must log in
first (`recipes/tiktok-login.md`) and should clean up after (`recipes/reinstall-tiktok.md`) — otherwise every post comes
from whichever account was already logged in.

Run one single test run first, then the batch; check each post landed on the right account.

## Ask
"Which accounts? One post each, or several? Same post for all or different ones? How long between accounts? Stop if one
fails?"
