---
id: reference/tools-run-requirements
generated: true
audience: both
---

# Tools — run-requirements (1)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## get_run_requirements
- does: Read safe run requirements using exactly one selector: presetVersionId selects a prompt preset version; strategyId selects visible authoring metadata; strategyPresetId selects the caller-owned current saved strategy preset; strategyPresetVersionId selects an exact caller-owned saved strategy preset version, including deprecated versions. Root pickers/scalars and calls[].path describe scoped selectors. Echo returned strategyPresetVersionId into preview/create for an immutable pin. Parent strategy visibility and preset ownership still apply to admins. Source-oriented: it stays masked for a source you cannot read, even with an own setup; non-owners of the source use get_release_run_inputs and get_release_input_options on the exact runnable release instead.
- input: query { presetVersionId?: string; strategyId?: string; strategyPresetId?: string; strategyPresetVersionId?: string }
- returns: RunRequirementsResponse
- flags: read-only
