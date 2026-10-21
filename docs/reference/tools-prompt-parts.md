---
id: reference/tools-prompt-parts
generated: true
audience: both
---

# Tools — prompt-parts (10)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_prompt_parts
- does: list prompt parts. Caller rights and exact pins apply.
- input: query { page?: integer; size?: integer; orderBy?: "name"|"createdAt"|"updatedAt"; orderDirection?: "asc"|"desc"; role?: "system"|"turn"|"postcheck"|"success_criteria"|"task"; isDefault?: boolean; includeArchived?: boolean }
- returns: PaginatedResponse_PromptPartResponse
- flags: read-only

## create_prompt_part
- does: create prompt part. Caller rights and exact pins apply.
- input: body CreatePromptPartRequest { body: string; description?: string|null; isDefault?: boolean|null; name: string; notes?: string|null; role: "system"|"turn"|"postcheck"|"success_criteria"|"task" }
- returns: PromptPartWithVersionsResponse
- flags: changes data

## get_prompt_part_lookups
- does: get prompt part lookups. Caller rights and exact pins apply.
- input: none
- returns: PromptPartLookupsResponse
- flags: read-only

## get_prompt_part
- does: get prompt part. Caller rights and exact pins apply.
- input: path { part_id: string }
- returns: PromptPartWithVersionsResponse
- flags: read-only

## update_prompt_part_metadata
- does: Destructive: update prompt part metadata. Caller rights and exact pins apply.
- input: path { part_id: string } · body UpdatePromptPartMetadataRequest { description?: string|null; name?: string|null }
- returns: PromptPartResponse
- flags: changes data · destructive: confirm with the user first

## archive_prompt_part
- does: Destructive: archive prompt part. Caller rights and exact pins apply.
- input: path { part_id: string } · body null | EmptyPromptPartRequest {  }
- returns: PromptPartResponse
- flags: changes data · destructive: confirm with the user first

## set_default_prompt_part
- does: Destructive: set default prompt part. Caller rights and exact pins apply.
- input: path { part_id: string } · body SetDefaultPromptPartRequest { isDefault: boolean }
- returns: PromptPartResponse
- flags: changes data · destructive: confirm with the user first

## unarchive_prompt_part
- does: unarchive prompt part. Caller rights and exact pins apply.
- input: path { part_id: string } · body null | EmptyPromptPartRequest {  }
- returns: PromptPartResponse
- flags: changes data

## create_prompt_part_version
- does: create prompt part version. Caller rights and exact pins apply.
- input: path { part_id: string } · body CreatePromptPartVersionRequest { argSignature?: ManualPromptSignatureRequest[]; body: string; notes?: string|null }
- returns: PromptPartWithVersionsResponse
- flags: changes data

## set_current_prompt_part_version
- does: Destructive: set current prompt part version. Caller rights and exact pins apply.
- input: path { part_id: string; version_number: string } · body null | EmptyPromptPartRequest {  }
- returns: PromptPartWithVersionsResponse
- flags: changes data · destructive: confirm with the user first
