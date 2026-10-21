---
id: reference/tools-bundle-catalog
generated: true
audience: both
---

# Tools — bundle-catalog (9)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_prompt_part_bundles
- does: list prompt part bundles. Caller rights and exact pins apply.
- input: query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string; slotKind?: string; includeArchived?: boolean }
- returns: PaginatedResponse_BundleResponse
- flags: read-only

## create_prompt_part_bundle
- does: create prompt part bundle. Caller rights and exact pins apply.
- input: body CreateBundleRequest { description?: string|null; initialVersionNotes?: string|null; name: string; parts: BundlePartInput[]; slotKind: string }
- returns: BundleWithVersionsResponse
- flags: changes data

## get_prompt_part_bundle
- does: get prompt part bundle. Caller rights and exact pins apply.
- input: path { bundle_id: string }
- returns: BundleWithVersionsResponse
- flags: read-only

## update_prompt_part_bundle_metadata
- does: Destructive: update prompt part bundle metadata. Caller rights and exact pins apply.
- input: path { bundle_id: string } · body UpdateBundleMetadataRequest { description?: string|null; name?: string|null }
- returns: BundleResponse
- flags: changes data · destructive: confirm with the user first

## archive_prompt_part_bundle
- does: Destructive: archive prompt part bundle. Caller rights and exact pins apply.
- input: path { bundle_id: string } · body null | EmptyBundleRequest {  }
- returns: BundleResponse
- flags: changes data · destructive: confirm with the user first

## unarchive_prompt_part_bundle
- does: unarchive prompt part bundle. Caller rights and exact pins apply.
- input: path { bundle_id: string } · body null | EmptyBundleRequest {  }
- returns: BundleResponse
- flags: changes data

## create_prompt_part_bundle_version
- does: create prompt part bundle version. Caller rights and exact pins apply.
- input: path { bundle_id: string } · body CreateBundleVersionRequest { notes?: string|null; parts: BundlePartInput[] }
- returns: BundleWithVersionsResponse
- flags: changes data

## set_current_prompt_part_bundle_version
- does: Destructive: set current prompt part bundle version. Caller rights and exact pins apply.
- input: path { bundle_id: string; version_number: string } · body null | EmptyBundleRequest {  }
- returns: BundleWithVersionsResponse
- flags: changes data · destructive: confirm with the user first

## batch_get_prompt_part_versions
- does: batch get prompt part versions. Caller rights and exact pins apply.
- input: body BatchGetPartVersionsRequest { versionIds: string[] }
- returns: PartVersionBatchResponse
- flags: read-only
