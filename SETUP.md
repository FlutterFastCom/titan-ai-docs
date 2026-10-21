# Setup

1. In Titan, create an API key (Settings → API keys). It starts with `tck_`.
2. In a terminal, set two variables (never commit them):
   ```
   export TITAN_API_KEY=tck_...
   export TITAN_MCP_URL=https://YOUR-TITAN-BACKEND/mcp
   ```
3. Open this folder in your AI:
   - Claude Code: `claude` (reads `CLAUDE.md` and `.mcp.json`)
   - Codex: copy `.codex/config.toml.example` to `.codex/config.toml`, then `codex` (reads `AGENTS.md`)
   - Claude Desktop, ChatGPT or other tools: upload the release zip and start with `START-HERE.md`

Then just ask: "What should I do now?"
