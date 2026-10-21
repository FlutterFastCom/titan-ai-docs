---
id: reference/tools-llm-adapters
generated: true
audience: both
---

# Tools — llm-adapters (2)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_adapters
- does: List all live supported adapters owned by the key holder, with provider, model, rate-limit and status details. Unpaged; use list_adapter_options for paged ID and label choices. No credentials.
- input: none
- returns: LlmAdaptersResponse
- flags: read-only

## list_adapter_options
- does: Pick an adapter ID from paged ID and label choices for the owner's live supported adapters. For unpaged provider, model, rate-limit and status details, use list_adapters. No credentials.
- input: query { page?: integer; size?: integer }
- returns: LlmAdapterOptionsResponse
- flags: read-only
