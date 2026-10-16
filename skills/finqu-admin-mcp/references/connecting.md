# Connecting an Admin MCP client

Point the client at `https://mcp.finqu.com/mcp` with a bearer access token. Transport is Streamable HTTP.

## Claude Desktop

Config file:

- macOS: `~/Library/Application Support/Claude/claude_desktop_config.json`
- Windows: `%APPDATA%\Claude\claude_desktop_config.json`

```json
{
  "mcpServers": {
    "finqu-admin": {
      "url": "https://mcp.finqu.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN"
      }
    }
  }
}
```

Restart Claude Desktop after saving. Tell the session to call `list_channels` before theme or navigation tools.

## Cursor and Codex

Create `.cursor/mcp.json` in the repository root:

```json
{
  "mcpServers": {
    "finqu-admin": {
      "url": "https://mcp.finqu.com/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_ACCESS_TOKEN"
      }
    }
  }
}
```

Operating rules for the coding agent:

1. Call `get_profile`, then `list_channels`, and store `channel_id`.
2. Theme, navigation, and page tools require `type` `webstore`.
3. Confirm destructive writes with the user before calling them.
4. On `{ "status": "error" }`, read `response.code`. For `revision_mismatch` or `release_missing`, refresh state before retrying.
5. Treat `tools/list` as the schema source. Do not invent arguments.

## Other clients

- VS Code (`.vscode/mcp.json`) uses a top-level `servers` object and `type: "http"`. It can complete OAuth in the MCP editor instead of a static bearer header.
- Gemini CLI uses `httpUrl` for Streamable HTTP. `url` selects the older SSE transport. Do not use SSE for Admin MCP.
- Cline uses `type: "streamableHttp"` and the same bearer header.

Clients that implement OAuth discovery can use the URL alone and follow the `401` `resource_metadata` challenge. See `authentication.md`.

## Full reference

- [Connecting agents](https://developers.finqu.com/apis-and-tools/mcp/connecting-agents.md.txt)
