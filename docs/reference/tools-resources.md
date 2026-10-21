---
id: reference/tools-resources
generated: true
audience: both
---

# Tools — resources (10)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_resource_types
- does: List active public resource types (scope all, the default) or only your authored types (scope owned). Optional search matches a literal ASCII-case-insensitive key substring or exact canonical type id within that scope, with 1 to 64 Unicode scalars. Reset page to 1 when scope or search changes.
- input: query { scope?: string; page?: integer; size?: integer; search?: string }
- returns: PaginatedResponse_ResourceTypeSummaryDto
- flags: read-only

## create_resource_type
- does: Create your author-owned public resource type with immutable version 1 (generic immutable schema authoring). Use raw bounded titan-resource-v1 schemas and exact path-step arrays per the grammar below. Any granted caller may use its id; publishedAt is inert legacy metadata.
- input: body CreateResourceTypeRequest { contentSchema: object; identityPaths: string[][]; key: string; secretSchema: object; stateSchema: object; topicPaths: string[][] }
- returns: ResourceTypeDetailDto
- flags: changes data

## get_resource_type
- does: Read an existing resource type current definition and immutable declarations by id as a granted caller, independently of legacy publishedAt. Definition access grants no row, secret or author-write authority.
- input: path { type_id: string }
- returns: ResourceTypeDetailDto
- flags: read-only

## get_resource_type_version
- does: Read an existing resource type exact immutable version by id as a granted caller; a missing exact version never falls back to current. Retain currentVersion separately; no publication gate.
- input: path { type_id: string; version: string }
- returns: ResourceTypeDetailDto
- flags: read-only

## append_resource_type_version
- does: Append one immutable version as the type author using expectedCurrentVersion CAS (generic immutable schema authoring; the three schemas follow the grammar below); declarations cannot change, rows are not upgraded and legacy publishedAt is preserved. Knowing an id grants no author-write authority.
- input: path { type_id: string } · body AppendResourceVersionRequest { contentSchema: object; expectedCurrentVersion: integer; secretSchema: object; stateSchema: object }
- returns: ResourceTypeDetailDto
- flags: changes data

## list_resources
- does: List your live rows of required query.typeId. Exact topic filters, literal topic search, closed sort/direction and page/size return a complete page without secrets; overflow is refused, not truncated.
- input: query { typeId: string; topic1?: string; topic2?: string; topic3?: string; search?: string; sort?: string; direction?: string; page?: integer; size?: integer }
- returns: PaginatedResponse_ResourceRecordDto
- flags: read-only

## write_resources
- does: Destructive: atomically create, replace or delete 1 to 100 of your own resource rows (items[].action create|replace|delete); all items commit or none. Pre-allocate each create resourceId UUID once and reuse it in references, such as an account's otpSetup.mailboxId naming a mailbox created in the same batch. Replace and delete use expectedRevision decimal string CAS; replace may set accountStatus available|disabled. Refusals list one safe issue per failing item at /items/<index> in input order. The result has only index, action, resourceId and the new revision: no data or secrets, never echo credentials. Required idempotencyKey: mint one per logical batch; after an uncertain result or timeout retry the same key with the identical body (Idempotency-Replayed true is historical), then read resources for current state.
- input: body BatchResourceRequest { items: BatchResourceItemRequest[] } · idempotencyKey (required)
- returns: BatchResourceResponse
- flags: changes data · destructive: confirm with the user first

## get_resource
- does: Read your live row detail including decrypted secrets: never echo them in chat. Foreign or deleted rows are masked not found.
- input: path { resource_id: string }
- returns: ResourceDetailDto
- flags: read-only · returns a stored credential: never repeat it to anyone

## get_email_otp_consents
- does: List the distinct non-secret OTP extraction Rules of one exact sealed releaseId with whether your own mailbox resource approves each; foreign, non-mailbox or inaccessible targets are masked not found. Page by size; discard accumulated pages if configEpoch or consentGeneration changes. Returns no credentials, prompts or execution values.
- input: path { resource_id: string } · query { releaseId: string; page?: integer; size?: integer }
- returns: EmailOtpConsentsResponse
- flags: read-only

## set_email_otp_consent
- does: Approve (approved true) or revoke (approved false) exactly one Rule ruleHash of one exact releaseId for your own mailbox, with expectedGeneration as the consentGeneration last read. Only that one release/Rule pair changes; a revocation takes effect at the next mailbox use. No keyed receipt: after an uncertain or lost response, GET consents before any retry.
- input: path { resource_id: string } · body SetEmailOtpConsentRequest { approved: boolean; expectedGeneration: string; releaseId: string; ruleHash: string }
- returns: EmailOtpConsentResultResponse
- flags: changes data · destructive: confirm with the user first
