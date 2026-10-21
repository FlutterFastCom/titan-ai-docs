---
id: reference/tools-preset-catalog
generated: true
audience: both
---

# Tools — preset-catalog (10)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## get_prompt_part_preset_version
- does: get prompt part preset version. Caller rights and exact pins apply.
- input: path { version_id: string }
- returns: PresetVersionResponse
- flags: read-only

## list_prompt_part_presets
- does: list prompt part presets. Caller rights and exact pins apply.
- input: query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string; includeArchived?: boolean }
- returns: PaginatedResponse_PresetResponse
- flags: read-only

## create_prompt_part_preset
- does: create prompt part preset (a prompt-composition preset, not a strategy execution setup). Caller rights and exact pins apply. Returns currentVersionRowId: use exactly that as an LLM step promptVersionId. On preset_catalog_conflict, read list_prompt_part_presets/get_prompt_part_preset and reconcile the intended create/update; do not retry unchanged or overwrite blindly.
- input: body CreatePresetRequest { description?: string|null; initialVersion: InitialPresetVersionBody; initialVersionNotes?: string|null; name: string }
- returns: PresetWithVersionsResponse
- flags: changes data

## get_prompt_part_preset
- does: get prompt part preset. Caller rights and exact pins apply.
- input: path { preset_id: string }
- returns: PresetWithVersionsResponse
- flags: read-only

## update_prompt_part_preset_metadata
- does: Destructive: update prompt part preset metadata. Caller rights and exact pins apply.
- input: path { preset_id: string } · body UpdatePresetMetadataRequest { description?: string|null; name?: string|null }
- returns: PresetResponse
- flags: changes data · destructive: confirm with the user first

## archive_prompt_part_preset
- does: Destructive: archive prompt part preset. Caller rights and exact pins apply.
- input: path { preset_id: string } · body null | EmptyPresetRequest {  }
- returns: PresetResponse
- flags: changes data · destructive: confirm with the user first

## count_prompt_part_preset_refs
- does: count prompt part preset refs. Caller rights and exact pins apply.
- input: path { preset_id: string }
- returns: PresetRefsCountResponse
- flags: read-only

## unarchive_prompt_part_preset
- does: unarchive prompt part preset. Caller rights and exact pins apply.
- input: path { preset_id: string } · body null | EmptyPresetRequest {  }
- returns: PresetResponse
- flags: changes data

## create_prompt_part_preset_version
- does: create prompt part preset version. Caller rights and exact pins apply.
- input: path { preset_id: string } · body CreatePresetVersionRequest { chatConfigAuthored: ChatConfigAuthoredBody; notes?: string|null; parts: PresetPartInput[]; toolSetVersionId?: string|null }
- returns: PresetWithVersionsResponse
- flags: changes data

## set_current_prompt_part_preset_version
- does: Destructive: set current prompt part preset version. Caller rights and exact pins apply.
- input: path { preset_id: string; version_number: string } · body null | EmptyPresetRequest {  }
- returns: PresetWithVersionsResponse
- flags: changes data · destructive: confirm with the user first
