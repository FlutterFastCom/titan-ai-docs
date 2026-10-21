---
id: reference/tools-tool-catalog
generated: true
audience: both
---

# Tools — tool-catalog (14)

> Generated from the Titan code — do not edit by hand. For the assistant only: never show tool names, arguments or codes to the user.


## get_tool_set_version
- does: Read one exact global tool-set version by id; any granted caller, independent of creator. Exact pins apply; reading grants no tool invocation right.
- input: path { version_id: string }
- returns: ToolSetVersionWithNameResponse
- flags: read-only

## list_tool_sets
- does: List the global granted tool-set catalog; any granted caller, independent of creator or owner. Reading grants no tool invocation right.
- input: query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string }
- returns: PaginatedResponse_ToolSetResponse
- flags: read-only

## create_tool_set
- does: Enabled authors create a tool set they own with its initial exact tool roster; API-key/MCP tool-set mutations stay target-owner-only even for administrator keys; cross-owner tool-set maintenance requires a current granted APP-session administrator. No keyed receipt or automatic retry; read state after an uncertain result.
- input: body CreateToolSetRequest { description?: string|null; name: string; notes?: string|null; toolIds: string[] }
- returns: ToolSetWithVersionsResponse
- flags: changes data

## get_tool_set
- does: Read one global tool set with its exact versions; any granted caller, independent of creator. Reading grants no tool invocation right.
- input: path { tool_set_id: string }
- returns: ToolSetWithVersionsResponse
- flags: read-only

## update_tool_set_metadata
- does: Destructive: enabled authors update their own tool-set name/description; API-key/MCP tool-set mutations stay target-owner-only even for administrator keys; cross-owner tool-set maintenance requires a current granted APP-session administrator. Owner and creator provenance are unchanged. No automatic retry; read state after an uncertain result.
- input: path { tool_set_id: string } · body UpdateToolSetMetadataRequest { description?: string|null; name?: string|null }
- returns: ToolSetResponse
- flags: changes data · destructive: confirm with the user first

## archive_tool_set
- does: Destructive: enabled authors archive their own tool set; API-key/MCP tool-set mutations stay target-owner-only even for administrator keys; cross-owner tool-set maintenance requires a current granted APP-session administrator. Retained versions and captured exact pins survive. No automatic retry; read state after an uncertain result.
- input: path { tool_set_id: string }
- returns: ToolSetResponse
- flags: changes data · destructive: confirm with the user first

## unarchive_tool_set
- does: Enabled authors restore their own archived tool set; API-key/MCP tool-set mutations stay target-owner-only even for administrator keys; cross-owner tool-set maintenance requires a current granted APP-session administrator. No automatic retry; read state after an uncertain result.
- input: path { tool_set_id: string }
- returns: ToolSetResponse
- flags: changes data

## create_tool_set_version
- does: Enabled authors create a new immutable version of their own tool set and make it current; API-key/MCP tool-set mutations stay target-owner-only even for administrator keys; cross-owner tool-set maintenance requires a current granted APP-session administrator. Existing exact pins never move. No keyed receipt or automatic retry; read state after an uncertain result.
- input: path { tool_set_id: string } · body CreateToolSetVersionRequest { notes?: string|null; toolIds: string[] }
- returns: ToolSetWithVersionsResponse
- flags: changes data

## set_current_tool_set_version
- does: Destructive: enabled authors choose the current version of their own tool set; API-key/MCP tool-set mutations stay target-owner-only even for administrator keys; cross-owner tool-set maintenance requires a current granted APP-session administrator. Existing exact pins never move. No automatic retry; read state after an uncertain result.
- input: path { tool_set_id: string; version_number: string }
- returns: ToolSetWithVersionsResponse
- flags: changes data · destructive: confirm with the user first

## list_tools
- does: tools list. Granted catalog read.
- input: query { page?: integer; size?: integer; orderBy?: string; orderDirection?: string; isEnabled?: boolean }
- returns: PaginatedResponse_ToolResponse
- flags: read-only

## get_tool
- does: tools get. Granted catalog read.
- input: path { tool_id: string }
- returns: ToolWithVersionsResponse
- flags: read-only

## admin_update_executable_tool
- does: Destructive: Administrator only; enable or disable an executable tool.
- input: path { tool_id: string } · body UpdateToolMetadataRequest { isEnabled: boolean }
- returns: ToolResponse
- flags: changes data · destructive: confirm with the user first · admin only: never offer to other users

## admin_create_executable_tool_version
- does: Administrator only; append immutable description/notes version. No automatic retry.
- input: path { tool_id: string } · body CreateToolVersionRequest { description: string; notes?: string|null }
- returns: ToolWithVersionsResponse
- flags: changes data · admin only: never offer to other users

## admin_set_current_executable_tool_version
- does: Destructive: Administrator only; select an existing executable tool version as current.
- input: path { tool_id: string; version_number: string } · body EmptyToolActionRequest {  }
- returns: ToolWithVersionsResponse
- flags: changes data · destructive: confirm with the user first · admin only: never offer to other users
