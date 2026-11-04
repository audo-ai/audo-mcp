# Installing the Audo MCP server

Audo's MCP server is hosted: there is nothing to install or build.

1. Add a remote MCP server named `audo` with the URL `https://audo.ai/mcp` (streamable HTTP).

   In a CLI client:

   ```sh
   claude mcp add --transport http --scope user audo https://audo.ai/mcp
   codex mcp add audo --url https://audo.ai/mcp
   gemini mcp add --transport http --scope user audo https://audo.ai/mcp
   copilot mcp add --transport http audo https://audo.ai/mcp
   ```

   In a JSON config:

   ```json
   { "mcpServers": { "audo": { "url": "https://audo.ai/mcp" } } }
   ```

   In Cline (`cline_mcp_settings.json`), also set the transport:

   ```json
   { "mcpServers": { "audo": { "type": "streamableHttp", "url": "https://audo.ai/mcp" } } }
   ```

2. The first time a tool runs, sign in at audo.ai when asked (OAuth). No API key is needed (headless clients can pass an API key from https://audo.ai/keys as a Bearer token).
3. Check it works: ask "Use Audo to show my account" (`get_account`).

Every tool has a free part each day. Docs: https://audo.ai/docs/mcp?utm_source=github&utm_medium=directory
