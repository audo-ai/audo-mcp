# Installing the Audo MCP server

Audo's MCP server is hosted: there is nothing to install or build.

1. Add a remote MCP server named `audo` with the URL `https://audo.ai/mcp` (streamable HTTP). For example, in a JSON config:

   ```json
   { "mcpServers": { "audo": { "url": "https://audo.ai/mcp" } } }
   ```

   In Cline (`cline_mcp_settings.json`), also set the transport:

   ```json
   { "mcpServers": { "audo": { "type": "streamableHttp", "url": "https://audo.ai/mcp" } } }
   ```

2. The first time a tool runs, sign in at audo.ai when asked (OAuth). No API key is needed.
3. Check it works: ask "Use Audo to show my account" (`get_account`).

Every tool has a free part each day. Docs: https://audo.ai/docs/mcp
