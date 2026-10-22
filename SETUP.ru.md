# Настройка

1. Войдите на https://app.titan.red и создайте API-ключ (**Настройки → API-ключи** → **Создать ключ**). Он начинается с
   `tck_`. Скопируйте его сразу — Titan больше его не покажет.
2. В терминале задайте ключ (никогда не коммитьте его и не вставляйте в файлы этой папки):
   ```
   export TITAN_API_KEY=tck_...
   ```
   Адрес MCP уже прописан: https://backend-production-585b.up.railway.app/mcp
   (задавайте `TITAN_MCP_URL` только если вам дали другой сервер Titan).
   В Windows (PowerShell): `$env:TITAN_API_KEY="tck_..."`
3. Откройте эту папку в своём ИИ:
   - Claude Code: `claude` (читает `CLAUDE.md` и `.mcp.json`)
   - Codex: скопируйте `.codex/config.toml.example` в `.codex/config.toml`, затем `codex` (читает `AGENTS.md`)
   - Claude Desktop, ChatGPT и другие: добавьте адрес MCP выше с заголовком `Authorization: Bearer <ваш ключ>`,
     загрузите архив релиза и начните с `START-HERE.md`

Дальше просто спросите: «Что мне делать сейчас?» Ассистент ответит по-русски.
