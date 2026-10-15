# Admin MCP authentication

Admin MCP uses the same OAuth 2.0 access tokens as the Admin REST API. There is no MCP-specific API key.

## Endpoints

| Role | URL |
|------|-----|
| MCP | `https://mcp.finqu.com/mcp` |
| Authorization | `https://account.finqu.com/oauth2/authorize` |
| Token | `https://account.finqu.com/oauth2/access_token` |
| Authorization server metadata | `https://account.finqu.com/.well-known/oauth-authorization-server` |
| Protected resource | `https://mcp.finqu.com/.well-known/oauth-protected-resource/mcp` |

Supported grants include `authorization_code`, `refresh_token`, and `client_credentials`. Public clients use PKCE `S256`.

## Discovery

1. Call `https://mcp.finqu.com/mcp` without `Authorization`.
2. The response is HTTP `401` with `WWW-Authenticate` and `resource_metadata` (RFC 9728).
3. Fetch the protected-resource document. It sets `resource` to `https://mcp.finqu.com/mcp` and `authorization_servers` to `https://account.finqu.com`.
4. Run the authorize and token steps against `https://account.finqu.com`.

The same protected-resource document is also served at `https://mcp.finqu.com/.well-known/oauth-protected-resource`.

Send the access token on every MCP request:

```http
Authorization: Bearer YOUR_ACCESS_TOKEN
```

`GET https://account.finqu.com/oauth2/resource` can include `merchant.endpoints.mcp`. Admin MCP clients still connect only to `https://mcp.finqu.com/mcp`.

## Scopes

Request the minimum REST scopes the agent needs. Write scopes include the matching read permission. All platform admin tools still appear in `tools/list` for a merchant OAuth token. A missing scope fails when the tool is called.

| Scope | What it gates |
|-------|----------------|
| `profile` | `list_channels` (also `sales_channel_read` / `sales_channel_write`). `get_profile` works on any merchant token |
| `catalog_read` / `catalog_write` | Products, relations, groups, manufacturers, taxes, deliverers, reviews |
| `inventories_read` / `inventories_write` | Inventory, transfers, warehouses |
| `customers_read` / `customers_write` | Customers, groups, segments, tags |
| `sales_read` / `sales_write` | Orders, refunds, order notes |
| `marketing_read` / `marketing_write` | Discounts, marketing messages, social (`webstore` and `pos`) |
| `themes_read` / `themes_write` | Theme files, sections, blocks, settings |
| `settings_read` / `settings_write` | Policies, merchant and billing address |
| `sales_channel_read` / `sales_channel_write` | `list_channels`, navigation, pages, articles |

Legacy aliases `products_*` → `catalog_*` and `orders_*` → `sales_*` still work. Staff role limits still apply: a token scope cannot exceed the user's admin permissions.

Partner access tokens on this URL do not receive the admin catalog.

## HTTP errors

| Status | When |
|--------|------|
| `401` | Missing or invalid `Authorization`, invalid token, inactive merchant, or a forwarded call from an unapproved OAuth client |
| `403` | The user is not a staff member of the merchant |
| `429` | Forwarded admin tool call exceeded the client rate limit (`Too many requests.`) |

A `401` body looks like `{ "error": "unauthorized", "message": "…" }`. Missing or foreign `channel_id` is an MCP tool error, not HTTP 401 or 403.

## Full reference

- [Admin MCP authentication](https://developers.finqu.com/apis-and-tools/mcp/authentication.md.txt)
- [OAuth guide](https://developers.finqu.com/get-started/authentication.md.txt)
