---
id: explain/concepts
audience: both
intents: [what is, what does it mean, explain, how does titan work]
tools: [list_resource_types, write_resources, create_strategy, save_definition, create_release, create_strategy_preset, create_run, play_run, preview_queue, create_and_start_queue]
sources:
  - be:src/adapters/http/mcp/presentation.rs
  - be:src/application/resources/records.rs
  - be:docs/business/features/shipped/F-STRATEGY-AUTHORING.md
verified_be: 92a0a9c1
---
# How Titan fits together (for you; explain only the plain column to the user)

| plain word for the user | what it really is | tools that touch it |
|---|---|---|
| your iPhones | devices connected through the Titan PC Agent on the user's computer | `list_devices`, `device_detail`, `get_device_summary` |
| accounts, descriptions, posts, prompts | **resources**: saved rows of a **resource type** (a schema). Built-in types: `account_emailset_v1` (social account + password + optional mailbox for login codes), the description type (one text `value`), `post` (a post with 1–9 media items), `user_prompt`, `email_mailbox_v1`, `email_otp_rule_v1`. Wi-Fi is NOT a resource — it has its own tools. | `list_resource_types`, `get_resource_type`, `list_resources`, `write_resources` |
| a strategy | a named automation: a tree of **steps**. Only three step kinds exist: `group` (a list of steps, can repeat), `llm` (an AI step that drives the phone with a set of tools, guided by a prompt), `call` (runs another strategy's exact saved version). There is no separate "wait" or "repeat" step — repetition is `defaultLoop` on a step, or the loop in the run settings. | `create_strategy`, `get_definition`, `save_definition`, `get_strategy_overview` |
| the strategy's AI instructions | **prompt parts** (versioned text, each with a role: system, turn, postcheck, success_criteria, task) assembled into a **prompt composition** (`create_prompt_part_preset`), optionally with a **tool set** (which phone actions the AI step may use). An `llm` step points at one exact composition version. | prompt-parts, preset-catalog, tool-catalog tools |
| save the changes / make them live | every save makes a new immutable **version**; a **release** seals one exact version so it can be run or shared. Saving alone does not make anything runnable. | `save_definition`, `create_release`, `list_releases` |
| share / make public | **shares** give named users run access to every eligible release; **publish** gives everyone run access. Neither shows them the strategy's inside. | `add_shares`, `remove_shares`, `publish_strategy`, `unpublish_strategy` |
| run settings | a **strategy preset** (setup): the user's saved choices for one exact release — which accounts, which texts, repeats. A setup stays pinned to its release; a new release needs a new setup. | `create_strategy_preset`, `get_preset_inputs`, `put_preset_inputs` |
| a run | one execution on one phone. Create it, then **play** it — creating alone does not start it. | `create_run`, `play_run`, `get_user_run`, `cancel_run` |
| a batch across phones | a **queue**: many runs spread over several phones, driven by a list (for example one run per account). | `preview_queue`, `create_and_start_queue`, `list_queues` |

## Who can do what
- **Anyone with access** can: see their phones, save their own accounts and content, use strategies they own, that were
  shared with them, or that are public, create run settings, start runs and batches, read their own results.
- **Strategy builders** (an administrator turned on "can build strategies" for them) can also create and change
  strategies, AI instructions and tool sets, release, share and publish. A builder's refusal reads
  `strategy_authoring_disabled`: tell the user an administrator must enable strategy building for their account — you
  cannot change it.
- **Administrators** have extra tools. Never use or mention them for anyone else.
- Someone running a shared or public strategy never sees its steps or its AI instructions, and cannot read the full
  AI transcript of its runs — only outcomes, activity and screenshots.

## Rules that save you from common mistakes
- Every ID you pass must be copied from a tool response — never built from a name.
- Pins never follow "the latest" by themselves: a step calls an exact version, a setup uses an exact release.
- Before retrying any write whose answer you did not get, read the current state first.
