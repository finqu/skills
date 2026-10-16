# UCP MCP tools and REST paths

Endpoint: `https://{shop-domain}/mcp` (Streamable HTTP, when enabled).

Every tool requires `meta.ucp-agent.profile`. `cancel_cart` also requires `meta.idempotency-key`.

Names below are the `#[McpTool]` names in `src/Ucp/Adapter/Mcp/`. `tools/list` on the connected server is still the schema source for arguments.

| Tool | REST |
|------|------|
| `search_catalog` | `POST /api/ucp/catalog/search` |
| `lookup_catalog` | `POST /api/ucp/catalog/lookup` |
| `get_product` | `POST /api/ucp/catalog/product` |
| `create_cart` | `POST /api/ucp/carts` |
| `get_cart` | `GET /api/ucp/carts/{id}` |
| `update_cart` | `PUT /api/ucp/carts/{id}` |
| `cancel_cart` | `POST /api/ucp/carts/{id}/cancel` |
| `create_checkout` | `POST /api/ucp/checkout-sessions` |
| `get_checkout` | `GET /api/ucp/checkout-sessions/{id}` |
| `update_checkout` | `PUT /api/ucp/checkout-sessions/{id}` |
| `complete_checkout` | `POST /api/ucp/checkout-sessions/{id}/complete` |
| `cancel_checkout` | `POST /api/ucp/checkout-sessions/{id}/cancel` |
| `get_order` | `GET /api/ucp/orders/{id}` |
| `search_locations` | `POST /api/ucp/locations/search` |
| `lookup_locations` | `POST /api/ucp/locations/lookup` |
| `ucp_describe` | Discovery profile (`GET /.well-known/ucp`) |

These names replace the former Storefront MCP tools (`search_products`, `add_cart_lines`, `update_cart_lines`, `remove_cart_lines`, `get_cart_checkout_url`).

`POST /api/ucp/oauth/token` and `POST /api/ucp/oauth/revoke` are form-urlencoded OAuth endpoints. They are not MCP tools.

## Lines and customizations

- `line_items[].item.id` is `variants[].id` from the product, not the parent product id.
- Read `metadata.customizations` on that variant. Each group has `id`, `title`, `required`, `input_type`, and `options`.
- On the line, `customizations[].id` is `options[].id`.
- For `select`, `radio`, and `checkbox`, `value` is the option id or its title.
- For `text` and `textarea`, `value` is the entered text. Apply `min_length` and `max_length` only when they are greater than 0.
- `update_cart` replaces the whole line set.

## Checkout

`create_checkout` / `POST /checkout-sessions`: pass `cart_id` and omit `line_items`. Returns `201`.

`update_checkout` restates `line_items`. Copy fulfillment ids from the session you just received.

`complete_checkout` only when `status` is `ready_for_complete`.

- Remaining balance: `status` becomes `requires_escalation` with message `payment_escalation`. Send the buyer to `continue_url`. Handler `com.finqu.hosted_checkout`. Do not collect a card.
- Zero balance: the session can complete without that handoff. `continue_url` may already be on the checkout body. It is the payment step only when status is `requires_escalation`.

`cancel_checkout` marks the session canceled and leaves the cart in place.

`Idempotency-Key` applies to cart create, cart cancel, checkout create, complete, and cancel.

## Orders and locations

`get_order` for a buyer's own order uses `Authorization: Bearer ucp_at_…` with `dev.ucp.shopping.order:read`. A channel API key is still accepted for channel-level order reads. Do not combine the two bearers.

`search_locations` and `lookup_locations` exist on the server. The profile advertises location capabilities only when the channel has pickup methods.

## Full reference

- [REST API](https://developers.finqu.com/reference/ucp/rest-api.md.txt)
- [MCP transport](https://developers.finqu.com/reference/ucp/mcp-transport.md.txt)
- [Getting started](https://developers.finqu.com/ai/agentic-commerce/getting-started.md.txt)
- [Shopping MCP schema](https://ucp.dev/2026-08-25/services/shopping/mcp.openrpc.json)
- [Common MCP schema](https://ucp.dev/2026-08-25/services/common/mcp.openrpc.json)
