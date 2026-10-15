# Inbound MCP Tools

Servers you register so Finqu AI can call your systems. Success Agent, Workers, and Playbooks use the imported tools.

This is inbound. External agents that operate Finqu admin connect the other way, to Admin MCP (`https://mcp.finqu.com/mcp`). Those OAuth sessions receive platform tools only. They do not receive servers you add here.

## Register

Work Hub → Settings → Connected Tools, or the Admin API:

```json
{
  "name": "Acme Warehouse ERP",
  "url": "https://mcp.acme-logistics.com/mcp",
  "protocol": "streamable_http",
  "auth_type": "bearer",
  "auth_config": { "token": "YOUR_SECRET_TOKEN" },
  "enabled": true
}
```

| Field | Values |
|-------|--------|
| `protocol` | `streamable_http` (preferred) or `sse` |
| `auth_type` | `bearer`, `oauth2`, or `none` |
| `auth_config` | `{ "token" }` for bearer. `{ "client_id", "client_secret", "token_url", "scopes" }` for `oauth2`. `{}` for `none` |

Finqu calls `tools/list` and imports the schemas. Tool names should be lowercase snake_case. Return JSON text. The call timeout is 30 seconds. Aim for 1–5 seconds, or return a job id and a status tool.

Assign imported names in Worker `tool_names` and Playbook `allowed_tools`.

## Full reference

- [Inbound MCP Tools](https://developers.finqu.com/ai/ai-commerce/mcp-tools.md.txt)
- [Admin MCP](https://developers.finqu.com/apis-and-tools/mcp/admin.md.txt)
