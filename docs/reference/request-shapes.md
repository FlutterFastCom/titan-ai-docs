---
id: reference/request-shapes
generated: true
audience: both
---

# Request shapes

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


The exact JSON each nested input type accepts. `?` or "(optional)" = may be left out; `a | b` = exactly one of them;
`"x"` = that literal string; `(1..64)` = allowed length, item count or value range. Send no fields beyond those listed.


## AiUsageScopeKindResponse
- "message" | "chat" | "step" | "run" | "queue" | "adapter" | "owner"

## ArgDefault
one of:
- InheritArg
- ArgScalar

## ArgInvocationCallRequest
- `entities`: map<string, integer (1..9223372036854775807)>
- `manual`: map<string, string>
- `path`: string[] (1..16 items)
- `resources` (optional): map<string, { rowId: string (uuid); schemaVersion: integer (1..4294967295); typeId: string (uuid) }>

## ArgInvocationRequest
- `calls` (optional): ArgInvocationCallRequest[] (0..4096 items)
- `entities` (optional): map<string, integer (1..9223372036854775807)>
- `manual` (optional): map<string, string>
- `resources` (optional): map<string, { rowId: string (uuid); schemaVersion: integer (1..4294967295); typeId: string (uuid) }>

## ArgScalar
one of:
- string
- number
- boolean

## ArgValueType
- "text" | "number" | "boolean"

## BatchAccountStatusSchema
- "available" | "disabled"

## BatchResourceItemRequest
one of:
- { action: "create"; data: object; resourceId: string (length 36..36); schemaVersion: integer (1..1000); secrets: object; typeId: string (length 36..36) }
- { accountStatus?: BatchAccountStatusSchema; action: "replace"; data: object; expectedRevision: string (length 0..19); resourceId: string (length 36..36); schemaVersion: integer (1..1000); secrets: object }
- { action: "delete"; expectedRevision: string (length 0..19); resourceId: string (length 36..36) }

## BudgetsBody
- `maxDurationMs`: integer (1..2147483647)
- `maxLlmTurns`: integer (1..2147483647)

## BundlePartInput
- `orderIndex`: integer (0..2147483647)
- `partTypeSlug`: string (length 1..100)
- `promptPartVersionId`: string (length 1..19)

## ChatConfigAuthoredBody
- `budgets`: BudgetsBody
- `postcheck`: PostcheckBody
- `schemaVersion` (optional): 2 | null
- `vision`: VisionBody

## CreateReleasePresetRequest
- `description` (optional): string | null
- `inputs` (optional): InputAssignmentDto[]
- `name`: string
- `releaseId`: string

## CreateRunSchema
one of:
- OwnedCreateRunSchema
- ReleaseCreateRunSchema

## CreateStrategyPresetRequest
one of:
- OwnerCreatePresetRequest
- CreateReleasePresetRequest

## DefinitionId
- string

## DefinitionLoopDto
- `delayMs` (optional): null | LoopDelay
- `maxDurationMs` (optional): integer | null (1..604800000)
- `times`: integer (1..1000)

## DefinitionNodeDto
one of:
- { defaultLoop?: DefinitionLoopDto; key?: string; kind: "group"; name: string; steps: DefinitionNodeDto[] }
- { autoRetries: integer (0..2147483647); bindings: map<string, ArgDefault>; defaultLoop?: DefinitionLoopDto; emailOtp?: EmailOtpStepDto; irreversible: boolean; key?: string; kind: "llm"; name: string; promptVersionId: DefinitionId; retryBackoffMs: (integer (0..9223372036854775807))[] }
- { bindings: map<string, ArgDefault>; defaultLoop?: DefinitionLoopDto; key?: string; kind: "call"; name: string; strategyId: DefinitionId; strategyVersionId: DefinitionId }

## DraftDefaultRequest
one of:
- { kind: "argument"; path?: (string (length 1..128))[] (0..16 items); selector: string; value: null | DraftValueRequest }
- { kind: "llmAdapter"; llmAdapterId: integer (1..9223372036854775807) }
- { kind: "userProxy"; userProxyId: string | null (length 0..19) }

## DraftDriverRequest
- `selector`: string (length 1..any)
- `values`: DraftDriverValueRequest[] (1..1000 items)

## DraftDriverValueRequest
- `assignedWorkerDeviceRowId` (optional): integer | null (1..9223372036854775807)
- `value`: DraftValueRequest

## DraftItemAddRequest
- `driver`: DraftItemDriverRequest

## DraftItemArgBindingRequest
- `entity`: string
- `id`: string (length 0..19)
- `path` (optional): (string (length 1..128))[] (0..16 items)

## DraftItemDeleteRequest
- `itemId`: string (length 0..19)

## DraftItemDriverRequest
- `entity` (optional): string
- `id` (optional): string (length 0..19)
- `kind` (optional): string
- `rowId` (optional): string (uuid)
- `schemaVersion` (optional): integer (1..4294967295)
- `typeId` (optional): string (uuid)

## DraftItemPutRequest
- `aiAdapterId` (optional): string | null (length 0..19)
- `args` (optional): DraftItemArgBindingRequest[] (0..64 items)
- `itemId`: string (length 0..19)
- `proxyId` (optional): string | null (length 0..19)
- `resourceInputs` (optional): DraftItemResourceInputRequest[] (0..64 items)

## DraftItemResourceInputRequest
- `inputId`: string (uuid)
- `value`: null | DraftItemResourceValueRequest

## DraftItemResourceValueRequest
- `kind`: string
- `rowId`: string (uuid)
- `schemaVersion`: integer (1..4294967295)
- `typeId`: string (uuid)

## DraftValueRequest
one of:
- { kind: "manual"; value: string (length 1..any) }
- { kind: "entity"; rowId: integer (1..9223372036854775807) }
- { kind: "resource"; rowId: string (uuid); schemaVersion: integer (1..4294967295); typeId: string (uuid) }

## DriverEditValueRequest
one of:
- { kind: "manual"; value: string (length 1..any) }
- { kind: "entity"; rowId: string (length 0..19) }
- { kind: "resource"; rowId: string (uuid); schemaVersion: integer (1..4294967295); typeId: string (uuid) }

## EditDriverItemRequest
- `assignedWorkerDeviceRowId`: string | null (length 0..19)
- `itemId`: string (length 0..19)
- `value`: DriverEditValueRequest

## EditDriverRequest
- `inputId` (optional): string (uuid)
- `items`: EditDriverItemRequest[] (0..1000 items)
- `kind` (optional): string
- `schemaVersion` (optional): integer (1..4294967295)
- `selector` (optional): string (length 1..any)
- `typeId` (optional): string (uuid)

## EditSetupRequest
- `presetId`: string | null (length 0..19)
- `strategyId`: string | null (length 0..19)
- `wifiConfigId`: string | null (length 0..19)

## EffectiveProxyRequest
one of:
- { mode: "direct" }
- { mode: "proxy"; proxyId: string (length 0..19) }

## EmailOtpRuleRefDto
- `revision`: string
- `rowId`: string

## EmailOtpStepDto
- `binding`: string
- `path`: string[]
- `ruleRef`: EmailOtpRuleRefDto

## EmptyBundleRequest
- object

## EmptyPresetRequest
- object

## EmptyPromptPartRequest
- object

## InheritArg
- `inherit`: string

## InitialPresetVersionBody
- `chatConfigAuthored`: ChatConfigAuthoredBody
- `parts`: PresetPartInput[] (0..100 items)
- `toolSetVersionId` (optional): string | null

## InputAssignmentDto
- `inputId`: string (uuid)
- `value`: InputValueDto

## InputValueDto
one of:
- { kind: "manual"; value: ArgScalar }
- { kind: "legacyEntity"; rowId: string }
- { kind: "resource"; rowId: string (uuid); schemaVersion: integer (1..2147483647); typeId: string (uuid) }

## LoopDelay
one of:
- integer (0..9223372036854775807)
- (integer (0..9223372036854775807))[] (2..2 items)

## ManualPromptSignatureRequest
- `label`: string (length 0..200)
- `name`: string (length 1..128)
- `required`: boolean
- `source`: ManualSignatureSource
- `valueType`: ValueTypeSchema

## ManualSignatureSource
- "manual"

## OwnedCreateRunSchema
- `argInvocation`: ArgInvocationRequest
- `deviceUdid`: string
- `llmAdapterId`: integer (-9223372036854775808..9223372036854775807)
- `manualMode` (optional): boolean
- `operationId` (optional): string | null (uuid)
- `presetId`: integer (-9223372036854775808..9223372036854775807)
- `wifiConfigId` (optional): integer | null (-9223372036854775808..9223372036854775807)

## OwnerCreatePresetRequest
- `authoringStatus` (optional): PresetAuthoringStatusSchema
- `description` (optional): string | null
- `name`: string
- `plan`: object
- `strategyId`: string
- `strategyVersionId`: string

## PhoneDeviceActionWire
one of:
- { type: "tap_pct"; x_pct: number (0..1); y_pct: number (0..1) }
- { duration_ms?: integer | null (10..10000); type: "long_press_pct"; x_pct: number (0..1); y_pct: number (0..1) }
- { duration_ms?: integer | null (10..10000); from_x_pct: number (0..1); from_y_pct: number (0..1); to_x_pct: number (0..1); to_y_pct: number (0..1); type: "swipe_pct" }
- { direction: PhoneScrollDirectionWire; distance_pct: number (any..0.8); duration_ms?: integer | null (10..10000); type: "scroll_pct" }
- { type: "press_home" }
- { bundle_id: string (length 1..256); type: "launch_app" }
- { bundle_id: string (length 1..256); type: "close_app" }
- { type: "accept_alert" }
- { type: "dismiss_alert" }
- { text: string (length 1..16384); type: "type_text" }

## PhoneScrollDirectionWire
- "up" | "down" | "left" | "right"

## PostcheckBody
- `enabled`: boolean

## PresetAuthoringStatusSchema
- "draft" | "shipped" | "abandoned"

## PresetPartInput
- `orderIndex`: integer (0..2147483647)
- `promptPartVersionId`: string (length 1..19)

## QueueArgRequest
one of:
- { mode: "fixed"; path?: string[] (0..16 items); selector: string (length 1..64); value: QueueValueRequest }
- { mode: "driver"; path?: string[] (0..16 items); selector: string (length 1..64); values: QueueDriverValueRequest[] (1..1000 items) }
- { mode: "cycle"; path?: string[] (0..16 items); selector: string (length 1..64); values: QueueValueRequest[] (1..1000 items) }

## QueueDriverValueRequest
- `assignedWorkerDeviceRowId` (optional): integer | null (-9223372036854775808..9223372036854775807)
- `value`: QueueValueRequest

## QueueValueRequest
one of:
- { kind: "manual"; value: string (length 1..any) }
- { kind: "entity"; rowId: integer (1..9223372036854775807) }
- { kind: "resource"; rowId: string (uuid); schemaVersion: integer (1..4294967295); typeId: string (uuid) }

## RecoveryBindingRequest
one of:
- { mode: "inherit" }
- { mode: "proxy"; proxyId: string (length 0..19) }

## ReleaseCreateRunSchema
- `deviceUdid`: string
- `inputs`: RunInputAssignmentRequest[]
- `llmAdapterId`: integer (-9223372036854775808..9223372036854775807)
- `manualMode` (optional): boolean
- `operationId` (optional): string | null (uuid)
- `releaseId`: string
- `strategyId`: string
- `wifiConfigId` (optional): integer | null (-9223372036854775808..9223372036854775807)

## ResourceBinding
- `fields`: ResourceFieldBinding[]
- `schemaVersion`: integer (1..2147483647)
- `toolSelectors` (optional): string[]
- `typeId`: string (uuid)

## ResourceFieldBinding
- `argument`: string
- `path`: string[]
- `section`: ResourceSection

## ResourceSection
- "data" | "secrets" | "state"

## RootDefinitionDto
- `arguments` (optional): map<string, ArgValueType>
- `key` (optional): string
- `kind`: RootGroupKind
- `name`: string
- `resourceBindings` (optional): map<string, ResourceBinding>
- `steps`: DefinitionNodeDto[]

## RootGroupKind
- "group"

## RunInputAssignmentRequest
- `inputId`: string (uuid)
- `value`: RunInputValueRequest

## RunInputValueRequest
one of:
- { kind: "manual"; value: ArgScalar }
- { kind: "legacyEntity"; rowId: string }
- { kind: "resource"; rowId: string (uuid); schemaVersion: integer (1..2147483647); typeId: string (uuid) }

## RunStepStatusFilter
- "all" | "running" | "success" | "failed" | "skipped" | "aborted"

## StrategyKindDto
- "claude_authoring" | "user_facing"

## UpdateUserProxyAuthRequest
one of:
- { kind: "none" }
- { kind: "userpass"; password?: string | null (length 1..any); username: string (length 1..any) }

## UserProxyAuthRequest
one of:
- { kind: "none" }
- { kind: "userpass"; password: string (length 1..any); username: string (length 1..any) }

## ValueTypeSchema
- "string" | "number" | "email" | "phone" | "secret" | "textarea"

## VisionBody
- `autoTrigger`: boolean
- `sparseThreshold`: integer (0..2147483647)

## WifiSecuritySchema
- "open" | "wpa2" | "wpa3"
