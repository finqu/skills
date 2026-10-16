---
name: finqu-admin-mcp
description: 'Finqu Admin MCP — operate a merchant store from an external agent over Streamable HTTP'
---

# Finqu Admin MCP

## When to use

Use this skill when:

- Connecting Claude Desktop, Cursor, Codex, or a custom agent to merchant admin operations
- Calling tools at `https://mcp.finqu.com/mcp`
- Choosing between Admin MCP, inbound MCP Tools, and storefront UCP

This skill is **outbound Admin MCP**: an external agent acts as the merchant. It is not:

- **Inbound MCP Tools** — servers a merchant registers so Success Agent, Workers, and Playbooks can call external systems. Use **finqu-ai-commerce**.
- **UCP** — customer shopping agents on `{shop-domain}/mcp`. Use **finqu-storefront-mcp**.

## Inputs required

- **OAuth access token**: the same tokens as the Admin REST API (`Authorization: Bearer …`)
- **Scopes**: the minimum REST scopes the tools need (`catalog_read`, `sales_read`, `themes_write`, and so on)
- **MCP client**: Streamable HTTP, tools only

## Procedure

### 0) Confirm the surface

1. Endpoint is `https://mcp.finqu.com/mcp`. Do not use a per-merchant host from `merchant.endpoints.mcp`.
2. Transport is Streamable HTTP. The server is stateless and exposes **tools** only (no resources or prompts).
3. OAuth sessions receive platform admin tools only. Merchant-configured inbound MCP Tools are not attached.

### 1) Authenticate

Admin MCP uses Admin REST OAuth tokens. There is no separate MCP API key.

Discovery for clients that implement OAuth:

1. `GET` or `POST` `https://mcp.finqu.com/mcp` without `Authorization`
2. Read `401` `WWW-Authenticate` for `resource_metadata`
3. Fetch `https://mcp.finqu.com/.well-known/oauth-protected-resource/mcp`
4. Authorize at `https://account.finqu.com` (authorization code + PKCE for public clients)

Read: `references/authentication.md`

### 2) Connect the client

Send the access token on every request:

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

Claude Desktop, Cursor, and Codex use the same HTTP server block. Cursor and Codex read `.cursor/mcp.json`.

Read: `references/connecting.md`

### 3) Start a session

1. Call MCP `tools/list`. That list is authoritative. Do not guess argument schemas from this skill or from memory.
2. Call `get_profile` for the signed-in user, merchant, and token scopes.
3. Call `list_channels` and keep the `channel_id` you will pass to later tools.
4. `get_profile` and `list_channels` return their objects directly. Other tools return `{ "status": "ok", "response": … }`.

`list_channels` needs `profile`, `sales_channel_read`, or `sales_channel_write`.

### 4) Call tools

Pass integer `channel_id` when the schema requires it. Theme, navigation, and page tools need a `webstore` channel. Marketing messages and social tools accept `webstore` or `pos`. If several channels match and the user did not name one, list them and ask.

Tool groups on the docs catalog are account, products (catalog, inventory, relations, groups, manufacturers), orders, customers, media, marketing, content, theme, and store setup. Scopes and staff permissions still decide which calls succeed. A missing scope fails at call time. Tools are not hidden from `tools/list`.

See [Admin MCP tools](https://developers.finqu.com/apis-and-tools/mcp/tools.md.txt).

### 5) Confirm writes

Ask the merchant before destructive or high-impact writes (delete, unpublish, refund, theme file changes, price changes). Show the current value and the proposed value. Wait for an explicit yes.

Read: `references/operating.md`

## Verification

- Unauthenticated `POST https://mcp.finqu.com/mcp` returns `401` with `WWW-Authenticate`
- Protected-resource metadata names `https://mcp.finqu.com/mcp` and authorization server `https://account.finqu.com`
- `tools/list` includes `get_profile` and `list_channels`
- `get_profile` is unwrapped; a catalog or order tool returns `{ status, response }`
- A tool that needs `channel_id` is called with an id from `list_channels`

## Failure modes / debugging

- **401 `unauthorized`**: missing or invalid bearer token, or inactive merchant. Unauthenticated requests include `WWW-Authenticate`
- **403**: the user is not staff on the merchant
- **429**: a forwarded admin tool call exceeded the OAuth client rate limit
- **Plain-text tool error for `channel_id`**: missing channel, or a channel that does not belong to the merchant. This is not the `{ status, response }` envelope
- **`{ status: "error", response: { message, code, suggestions } }`**: `release_missing`, `revision_mismatch`, `write_failed`, and similar. Refresh state before retrying
- **`insufficient_scope` on `list_channels`**: token lacks `profile`, `sales_channel_read`, and `sales_channel_write`
- **Inbound tools missing**: expected. OAuth sessions do not receive merchant MCP Tools

## Escalation

- See [Admin MCP](https://developers.finqu.com/apis-and-tools/mcp/admin.md.txt)
- See [Connecting agents](https://developers.finqu.com/apis-and-tools/mcp/connecting-agents.md.txt)
- See [Best practices](https://developers.finqu.com/apis-and-tools/mcp/best-practices.md.txt)
- See [Authentication](https://developers.finqu.com/apis-and-tools/mcp/authentication.md.txt)
- See [Tools](https://developers.finqu.com/apis-and-tools/mcp/tools.md.txt)
- See [MCP overview](https://developers.finqu.com/apis-and-tools/mcp/overview.md.txt)
