# Setup

1. Sign in at https://app.titan.red and create an API key (**Settings → API keys** → **Create key**). It starts with
   `tck_`. Copy it once — Titan will not show it again.
2. In a terminal, set the key (never commit it or paste it into a file in this folder):
   ```
   export TITAN_API_KEY=tck_...
   ```
   The MCP address is already filled in: https://backend-production-585b.up.railway.app/mcp
   (set `TITAN_MCP_URL` only if you were given a different Titan server).
   On Windows (PowerShell): `$env:TITAN_API_KEY="tck_..."`
3. Open this folder in your AI:
   - Claude Code: `claude` (reads `CLAUDE.md` and `.mcp.json`)
   - Codex: copy `.codex/config.toml.example` to `.codex/config.toml`, then `codex` (reads `AGENTS.md`)
   - Claude Desktop, ChatGPT or other tools: add the MCP address above with the header
     `Authorization: Bearer <your key>`, upload the release zip and start with `START-HERE.md`

Then just ask, in English or Russian: "What should I do now?" / «Что мне делать сейчас?»
