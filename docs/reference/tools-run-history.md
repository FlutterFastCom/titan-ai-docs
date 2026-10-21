---
id: reference/tools-run-history
generated: true
audience: both
---

# Tools — run-history (11)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## list_user_runs
- does: list user runs. Owned safe history.
- input: query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string; activeOnly?: boolean; presetId?: string; strategyId?: string; deviceUdid?: string; status?: string; search?: string }
- returns: PaginatedResponse_UserRunResponse
- flags: read-only

## get_user_run_chat
- does: Read one run chat you ran. transcript=full: you wholly own the strategy source, and chatSetup/adapterSnapshot/argInvocation are raw retained values that can include your own stored credentials. transcript=unavailable: another author's source; only the safe outcome remains. Use read_run_transcript for the complete bounded transcript.
- input: path { chat_id: string }
- returns: ChatDetailResponse
- flags: read-only

## list_user_run_chat_messages
- does: List messages of a run chat you ran. With transcript=full, prompts, replies and raw tool arguments/results are retained values that can include your own stored credentials; transcript=unavailable returns only safe rows. A large row page can exceed a byte bound: use read_run_transcript instead.
- input: path { chat_id: string } · query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string }
- returns: RunChatMessagesPageResponse
- flags: read-only

## get_user_message_screenshot
- does: Read the stored historical screenshot of one message in a run chat you ran, as one JPEG image block plus chatId/messageId/mimeType metadata. It is not a live frame. Absent, foreign and oversized images are typed refusals.
- input: path { chat_id: string; message_id: string }
- returns: MessageScreenshotResponse
- flags: read-only

## read_run_transcript
- does: Read the complete retained transcript of a run chat you ran, in resumable fragments: the setup record first, then each message by messageId; offset counts Unicode scalars. Concatenate fragments of one kind/messageId by offset; pass nextCursor until it is null. Pages fit query.size and query.maxBytes. transcript=unavailable means the strategy source is not wholly yours: do not debug that author's prompts. Raw retained values can include your own stored credentials. Screenshots are referenced, never inline: use get_user_message_screenshot. transcript_changed means start again without a cursor.
- input: path { chat_id: string } · query { cursor?: string; size?: integer; maxBytes?: integer }
- returns: RunTranscriptPageResponse
- flags: read-only · returns a stored credential: never repeat it to anyone

## list_run_device_options
- does: List the distinct devices (stored UDID plus your own current label, or null) that appear in runs you ran; searchable and paginated. Historical UDIDs remain when inventory is gone. Narrows your run history only: it claims no availability or eligibility and grants no execution permission.
- input: query { page?: integer; size?: integer; search?: string }
- returns: PaginatedResponse_RunDeviceOptionResponse
- flags: read-only

## list_run_strategy_options
- does: List the distinct strategies (id and current name) that appear in runs you ran; searchable and paginated. Membership comes from your own runs, not strategy visibility. Narrows your run history only: it grants no strategy source or execution permission.
- input: query { page?: integer; size?: integer; search?: string }
- returns: PaginatedResponse_RunStrategyOptionResponse
- flags: read-only

## get_user_run
- does: Read one run you ran with its outcome, failureClass and transcript availability (full when you wholly own the strategy source; unavailable otherwise). Full views can include your own stored credentials in retained values.
- input: path { run_id: string }
- returns: UserRunDetailResponse
- flags: read-only

## list_activity
- does: list activity. Owned safe history.
- input: path { run_id: string } · query { page?: integer; size?: integer; stepRecordId?: string }
- returns: PaginatedResponse_RunActivityResponse
- flags: read-only

## list_user_run_passes
- does: list user run passes. Owned safe history.
- input: path { run_id: string } · query { page?: integer; size?: integer }
- returns: PaginatedResponse_RunPassResponse
- flags: read-only

## list_user_run_steps
- does: List the attempts/steps of a run you ran with their run chats. With transcript=full, retained values can include your own stored credentials; read each chat with read_run_transcript.
- input: path { run_id: string } · query { page?: integer; size?: integer; search?: string; status?: RunStepStatusFilter; statuses?: string }
- returns: UserRunStepsPageResponse
- flags: read-only
