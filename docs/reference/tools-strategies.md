---
id: reference/tools-strategies
generated: true
audience: both
---

# Tools — strategies (37)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_user_strategies
- does: List safe strategy metadata with nullable exact current versionId/versionNumber, affirmative isArchived, a nullable newest eligible (sealed, non-withdrawn) releaseId and truthful owner/access markers plus each row's relation (mine, shared-with-me, public, private), actual root publication marker isPublished and current canManage hint. Granted non-admins discover their own, shared-with-them and every current public strategy automatically, with an optional source filter; current admins discover all users' strategies, including private strategies. For either audience absent or empty source keeps the full admitted base; nonempty owned,shared,public CSV selects exclusive Mine/Shared/Public classes in that precedence and excludes admin private roots. Optional ownerIds (at most 100 full owner UUIDs) only narrows the visible results. canManage is a fresh hint that never replaces mutation admission. Ordinary callers mutate only actually owned roots with fresh author permission; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor. Metadata reads do not require author permission.
- input: query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string; includeArchived?: boolean; complexity?: string; search?: string; source?: string; ownerIds?: string }
- returns: PaginatedResponse_UserStrategyListItemResponse
- flags: read-only

## list_strategy_owners
- does: List paged distinct owner user IDs from strategies visible under your current rights: mine, shared and all current public for ordinary callers; global strategy owners for current admins. Exact full owner-ID search only, no user/profile enumeration. Read permission requires granted access, not author capability.
- input: query { page?: integer; size?: integer; includeArchived?: boolean; search?: string }
- returns: PaginatedResponse_StrategyOwnerOptionResponse
- flags: read-only

## get_user_strategy
- does: Read safe strategy metadata by known ID under current rights, including nullable exact current versionId/versionNumber, affirmative isArchived, relation, isPublished and a current canManage hint. releaseId selects a distinct same-root sealed release; current version identity and hints grant no later mutation or run authority. Current administrators may read safe metadata across owners; ordinary keys retain their existing metadata visibility. This response exposes no private definition or execution values. Ordinary definition/overview source reads and source mutations stay actual-root-owner scoped (mutations also need fresh author permission); a current granted administrator has owner-equivalent strategy source rights on any owner's strategy through a session, API key or MCP, recorded as the real actor. Granted metadata rights do not require author permission.
- input: path { strategy_id: string }
- returns: UserStrategyListItemResponse
- flags: read-only

## get_strategy_overview
- does: View a strategy source for its actual owner, or for a current granted administrator with owner-equivalent strategy source and redacted-review rights through a session, API key or MCP, selecting current or an exact canonical versionNumber. Ordinary foreign shared/public roots remain masked. The current header may differ from the selected version. Returns the real immutable tree and existing permitted exact called definitions with explicit unavailable omissions. Ordinary prompt source is caller-owned under the existing whole-preset/member admission; a current granted administrator may read referenced exact preset and member-part source across owners only within this strategy's admitted closure. Global exact tool display pins remain available. View-only: no execution, credential or foreign resource/setup values and no standalone foreign prompt-catalog rights. Finite byte/cardinality/depth limits apply; complete compact JSON is capped at 6291456 bytes with no truncation.
- input: path { strategy_id: string } · query { versionNumber?: string; view?: string; presetId?: string; presetVersionId?: string }
- returns: StrategyOverviewReadResponse
- flags: read-only

## list_user_presets
- does: List safe saved execution-setup metadata and exact source pins with current/deprecated filtering. Ordinary lists omit both view and strategyId; review mode supplies view=review AND strategyId together and keeps its own source-review admission. Granted non-admins see their own setups; current admins see every user's setup metadata. Global metadata access does not expose or authorize another user's plan, inputs or mutations. Caller-owned setup and run operations retain granted runner admission without author permission.
- input: query { view?: string; strategyId?: string; page?: integer; size?: integer; orderBy?: string; orderDirection?: string; includeArchived?: boolean; search?: string; versionFilter?: string }
- returns: StrategyPresetListReadResponse
- flags: read-only

## get_user_preset
- does: Read safe saved execution-setup metadata by ID. A current admin may read another user's setup metadata; for non-admins, foreign and missing IDs remain masked. No foreign execution plan or input values are exposed or authorized. Caller-owned setup and run operations retain granted runner admission without author permission.
- input: path { preset_id: string }
- returns: UserStrategyPresetResponse
- flags: read-only

## create_strategy
- does: create strategy. Caller rights and exact pins apply.
- input: body CreateStrategyRequest { description?: string|null; kind?: null | StrategyKindDto; name: string }
- returns: StrategyResponse
- flags: changes data

## update_strategy_metadata
- does: Destructive: update strategy metadata. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string } · body UpdateStrategyMetadataRequest { description?: string|null; name: string }
- returns: StrategyResponse
- flags: changes data · destructive: confirm with the user first

## add_to_list
- does: add to list. Caller rights and exact pins apply.
- input: path { strategy_id: string }
- returns: ListEntryResponse
- flags: changes data

## archive_strategy
- does: Destructive: archive strategy. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string }
- returns: StrategyResponse
- flags: changes data · destructive: confirm with the user first

## hide_strategy
- does: Destructive: hide strategy. Caller rights and exact pins apply.
- input: path { strategy_id: string }
- returns: ListEntryResponse
- flags: changes data · destructive: confirm with the user first

## publish_strategy
- does: publish strategy: turn on the strategy public switch (no release selector); every eligible sealed release becomes runnable by any granted caller. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string }
- returns: StrategyResponse
- flags: changes data

## create_release
- does: create release: seal one exact saved version; every exact called version needs its own sealed, non-withdrawn release. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string } · body CreateReleaseRequest { strategyVersionId: string }
- returns: ReleaseResponse
- flags: changes data

## withdraw_strategy_release
- does: Destructive: withdraw one exact sealed release (actual owner with author capability, or a current granted administrator); body absent or {}. Repeating keeps the first withdrawnAt. The release leaves default lists and refuses every fresh selection/Start, including saved setups pinned to it; admitted runs and history keep captured evidence, recipients and the public switch are unchanged, and nothing is repinned or retried automatically. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string; release_id: string }
- returns: ReleaseResponse
- flags: changes data · destructive: confirm with the user first

## restore_strategy_release
- does: Restore one exact withdrawn release (actual owner with author capability, or a current granted administrator); body absent or {}. Requires an active strategy and every exact called release in its original manifest to be sealed and not withdrawn; a withdrawn child refuses with strategy_unavailable and is never restored automatically. Never reseals, rebases a setup or resumes a queue. Find withdrawn release IDs with list_releases query.includeWithdrawn=true. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string; release_id: string }
- returns: ReleaseResponse
- flags: changes data · destructive: confirm with the user first

## restore_strategy
- does: restore strategy. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string }
- returns: StrategyResponse
- flags: changes data

## set_reusable
- does: set reusable. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string } · body ReusableReleaseRequest { releaseId: string; reusable: boolean }
- returns: ReleaseResponse
- flags: changes data

## add_shares
- does: add shares: bulk strategy-level recipients (userIds, no release selector); recipients may run every eligible sealed release, selected by releaseId. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string } · body AddSharesRequest { userIds: string[] }
- returns: ShareMutationResponse
- flags: changes data

## remove_shares
- does: Destructive: remove shares: bulk strategy-level recipient removal (userIds, no release selector); admitted runs keep captured evidence. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string } · body RemoveSharesRequest { userIds: string[] }
- returns: ShareMutationResponse
- flags: changes data · destructive: confirm with the user first

## show_strategy
- does: show strategy. Caller rights and exact pins apply.
- input: path { strategy_id: string }
- returns: ListEntryResponse
- flags: changes data

## unpublish_strategy
- does: Destructive: unpublish strategy: turn off the strategy public switch (no release selector); strategy recipients keep their rights. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string }
- returns: StrategyResponse
- flags: changes data · destructive: confirm with the user first

## create_strategy_preset
- does: create strategy preset. Caller rights and exact pins apply.
- input: body CreateStrategyPresetRequest {  }
- returns: StrategyPresetResponse
- flags: changes data

## get_definition
- does: get definition. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string }
- returns: DefinitionResponse
- flags: read-only

## save_definition
- does: Destructive: save definition. Caller rights and exact pins apply. Body: expectedVersionId (required, nullable: null only for a never-saved definition, otherwise the exact version id from this strategy's own get_definition) plus definition. Node keys: omit key for every new root or descendant so the server allocates it; never invent keys or UUIDs. Keep a returned key only for a node under the same parent in this strategy's previous definition; a reparented node is new and omits key. Check /definition/key and /definition/steps/<index>/key recursively. Root arguments is optional (default {}) but must be an object when present. LLM promptVersionId is create_prompt_part_preset currentVersionRowId (prompt part -> composition preset), never a prompt-part version id. Minimal definition: {"kind":"group","name":"First strategy","arguments":{},"steps":[{"kind":"llm","name":"Step","promptVersionId":"<composition currentVersionRowId>","bindings":{},"autoRetries":0,"retryBackoffMs":[],"irreversible":false}]}; replace the bracketed value with an actual returned canonical composition version id. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string } · body SaveDefinitionRequest { definition: RootDefinitionDto; expectedVersionId: string|null }
- returns: DefinitionResponse
- flags: changes data · destructive: confirm with the user first

## list_releases
- does: list releases: every eligible (sealed, non-withdrawn) release newest first, with nullable withdrawnAt; select one by releaseId. query.includeWithdrawn=true is the owner/current-admin management view that also lists withdrawn seals (never drafts); for anyone else it is masked as strategy_not_found, and a listed receipt is not run permission. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string } · query { page?: integer; size?: integer; includeWithdrawn?: boolean }
- returns: PaginatedResponse_ReleaseResponse
- flags: read-only

## list_shares
- does: list shares: the strategy-level recipients. Caller rights and exact pins apply. Ordinary keys stay actual-root-owner scoped; a current granted administrator has owner-equivalent strategy rights on any owner's strategy through a session, API key or MCP, recorded as the real actor.
- input: path { strategy_id: string } · query { page?: integer; size?: integer; search?: string; lookupUserId?: string }
- returns: ShareReadResponse
- flags: read-only

## get_preset_template
- does: get preset template. Caller rights and exact pins apply.
- input: query { strategyId: string; strategyVersionId: string }
- returns: TemplateResponse
- flags: read-only

## get_preset_inputs
- does: get preset inputs. Caller rights and exact pins apply.
- input: path { preset_id: string }
- returns: SetupInputsResponse
- flags: read-only

## put_preset_inputs
- does: Destructive: put preset inputs. Caller rights and exact pins apply.
- input: path { preset_id: string } · body PutInputsRequest { expectedPresetVersionId: string; inputs: InputAssignmentDto[] }
- returns: SetupInputsResponse
- flags: changes data · destructive: confirm with the user first

## get_preset_plan
- does: get preset plan. Caller rights and exact pins apply. Source-oriented full-plan read: owning the execution setup alone grants no foreign source access, so strategy_preset_not_found (missing or inaccessible, masked) can occur for an own setup on an unreadable source. Use get_user_preset/list_user_presets for safe metadata and get_release_run_inputs/get_release_input_options to run; never fetch prompts or source.
- input: path { preset_id: string } · query { presetVersionId?: string }
- returns: PlanResponse
- flags: read-only

## save_preset_plan
- does: Destructive: save preset plan. Caller rights and exact pins apply.
- input: path { preset_id: string } · body SavePlanRequest { expectedVersionId: string; plan: object }
- returns: PlanResponse
- flags: changes data · destructive: confirm with the user first

## get_release
- does: get release. Safe exact-release rights only.
- input: path { release_id: string }
- returns: ReleaseMetadataResponse
- flags: read-only

## get_release_input_options
- does: get release input options. Safe exact-release rights only.
- input: path { release_id: string } · query { inputId: string; page?: integer; size?: integer }
- returns: PaginatedResponse_InputOptionDto
- flags: read-only

## get_release_run_inputs
- does: get release run inputs. Safe exact-release rights only.
- input: path { release_id: string }
- returns: RunInputsResponse
- flags: read-only

## update_strategy_preset
- does: Destructive: update strategy preset. Caller rights and exact pins apply.
- input: path { preset_id: string } · body UpdateStrategyPresetRequest { authoringStatus?: PresetAuthoringStatusSchema; description?: string|null; name?: string }
- returns: StrategyPresetResponse
- flags: changes data · destructive: confirm with the user first

## archive_strategy_preset
- does: Destructive: archive your own strategy preset, including an outdated pin. Saved version pins are unchanged; metadata and whole-plan edits of outdated presets remain refused. Read current state before retrying an uncertain result.
- input: path { preset_id: string } · body EmptyStrategyPresetActionRequest {  }
- returns: StrategyPresetResponse
- flags: changes data · destructive: confirm with the user first

## restore_strategy_preset
- does: Restore your own strategy preset, including an outdated pin, without changing saved version pins or granting edit/run permission. Read current state before retrying an uncertain result.
- input: path { preset_id: string } · body EmptyStrategyPresetActionRequest {  }
- returns: StrategyPresetResponse
- flags: changes data
