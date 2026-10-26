---
id: reference/tools-ai-usage
generated: true
audience: both
---

# Tools — ai-usage (2)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## get_ai_usage_summary
- does: Read your own recorded AI request usage evidence for exactly one scope (message, chat, native step attempt, run, queue, adapter or owner): known token sums with per-category reported and missing request counts, request outcome and completeness counts, and coverage. This is usage evidence, never provider billing certainty; history before accounting began is unavailable, not zero. Owner and adapter scopes need a from/to window of at most 31 days. No credentials.
- input: query { scope: AiUsageScopeKindResponse; scopeId?: string; from?: string; to?: string }
- returns: AiUsageSummaryResponse
- flags: read-only

## list_ai_usage_requests
- does: List your own recorded AI requests for exactly one scope, newest first, with per-request outcome, completeness and reported token counters, paged by page and size. This is usage evidence, never provider billing certainty; unreported counters stay null rather than zero. Owner and adapter scopes need a from/to window of at most 31 days. No credentials or response content.
- input: query { scope: AiUsageScopeKindResponse; scopeId?: string; from?: string; to?: string; page?: integer; size?: integer }
- returns: AiUsageRequestsResponse
- flags: read-only
