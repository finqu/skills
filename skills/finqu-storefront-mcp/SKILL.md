---
name: finqu-storefront-mcp
description: 'Finqu Agentic Commerce — connect AI shopping agents to stores via UCP (REST or MCP transport)'
---

# Finqu Storefront MCP (UCP)

## When to use

Use this skill when:

- Building an AI shopping assistant that transacts on one Finqu store
- Connecting an MCP client to `https://{shop-domain}/mcp`
- Calling UCP REST at `https://{shop-domain}/api/ucp`
- Linking a buyer and reading that buyer's orders
- Enabling or verifying UCP on a merchant sales channel

This skill is **customer-facing Agentic Commerce**. For merchant admin agents at `https://mcp.finqu.com/mcp`, use **finqu-admin-mcp**. For Success Agent, Workers, Playbooks, and inbound MCP Tools, use **finqu-ai-commerce**.

## Inputs required

- **Store domain**: the sales channel storefront host
- **UCP enabled**: channel settings, and MCP server enabled when you use `/mcp`
- **Shopping auth**: channel API key (`fq_secret_…`), signed requests, or anonymous access when the merchant allows it
- **Buyer auth** (order reads and linked checkout): access token `ucp_at_…` from identity linking
- **Platform profile URL**: `https://your-platform.example/.well-known/ucp`

## Procedure

### 0) Discover the store

1. `GET https://{shop-domain}/.well-known/ucp` (public)
2. Confirm `ucp.version` is `2026-08-25`
3. Treat `ucp.capabilities` as what this store advertises. Negotiation is the intersection with your platform profile
4. Choose REST (`/api/ucp`, always when UCP is on) or MCP (`/mcp`, when the merchant enables it)

Read: `references/ucp-fundamentals.md`

### 1) Authenticate with one bearer

One `Authorization` header, one token. Do not send `fq_secret_…` and `ucp_at_…` together.

| Presented bearer | Tier | Use |
|------------------|------|-----|
| `fq_secret_…` | Token (channel API key) | Catalog, cart, checkout, and channel-level order reads |
| `ucp_at_…` | User (buyer) | The linked customer's orders and optional checkout attach |
| Signature headers | Signed | When the merchant allows signed access and no bearer above is present |
| None | Anonymous | Catalog and cart only, when the merchant allows it |

A bad `ucp_at_…` is `401` `invalid_token`. It does not fall through to anonymous. The body `code` is `identity_required`. `WWW-Authenticate` includes `resource_metadata="https://{shop-domain}/.well-known/oauth-protected-resource"`.

Read: `references/authentication.md`

### 2) Shop

Send `UCP-Agent: profile="https://your-platform.example/.well-known/ucp"` on REST calls. On MCP, pass the same URL in `meta.ucp-agent.profile` on every tool call.

1. Search with `POST /api/ucp/catalog/search` (`search_catalog`). Open a product with `POST /api/ucp/catalog/product` (`get_product`).
2. Buy a variant: `line_items[].item.id` is `variants[].id`.
3. `customizations[].id` is an **option** id (`metadata.customizations[].options[].id`), not the group id. Required groups need a selection. For `text` and `textarea`, honor `min_length` and `max_length` only when they are greater than 0.
4. `POST /api/ucp/carts` (`create_cart`) with `Idempotency-Key`. Cart updates replace the full `line_items` set.

Read: `references/available-tools.md`

### 3) Check out

1. `POST /api/ucp/checkout-sessions` with `cart_id`. Omit `line_items`. MCP: `create_checkout`. Returns `201`.
2. `PUT /api/ucp/checkout-sessions/{id}` restates `line_items` and sets buyer, fulfillment, and discounts. Copy method, destination, group, and option ids from the session. MCP: `update_checkout`.
3. Call complete only when `status` is `ready_for_complete`. MCP: `complete_checkout`.
4. When a balance remains, `complete` sets `status` to `requires_escalation` and the buyer must open `continue_url`. Payment handler is `com.finqu.hosted_checkout`. The profile `checkout_url` is not that session.
5. A zero balance (including a fully covered gift card) can finish without the handoff. The checkout body may already include `continue_url`. Escalation is what makes it the payment step.
6. `identity_optional` means guest checkout is allowed. It is not a failure.

### 4) Link the buyer before a buyer order read

Public client: authorization code + PKCE `S256`. `client_id` is the platform profile URL. No client secret.

Scopes: `dev.ucp.shopping.order:read` is gated (required for buyer order reads). `dev.ucp.shopping.checkout:manage` is optional (attaches checkout to the customer).

Use `Authorization: Bearer ucp_at_…` for the buyer's own order (`GET /api/ucp/orders/{id}`, MCP `get_order`). A channel API key can still read orders in `OrderService`. It is not rejected. Do not use it when the task is the linked buyer's own order.

A valid buyer token without `dev.ucp.shopping.order:read` is `403` `insufficient_scope`. The challenge `resource_metadata` points at `/.well-known/oauth-protected-resource`.

Read: `references/identity-linking.md`

### 5) Enable UCP (merchants)

In channel **Settings → API**: enable UCP, optionally the MCP server, set agent access, and create a channel API key.

Read: `references/setup.md` and `references/building-ai-assistants.md`

## Verification

- Discovery returns `ucp.version` `2026-08-25`, services, and capabilities
- `GET /api/ucp` with a channel key returns `"protocol": "ucp"` and `"status": "ok"`
- MCP `tools/list` includes the shopping tools through `complete_checkout`, `get_order`, location tools, and `ucp_describe`
- A cart line uses a variant id. A customization uses an option id
- Complete with a remaining balance returns `requires_escalation` and `continue_url`
- Buyer order read uses `ucp_at_…`, not a combined Authorization header

## Failure modes / debugging

- **404 on discovery**: UCP is off for this domain — see [Activating UCP](https://developers.finqu.com/ai/agentic-commerce/activating-ucp.md.txt)
- **401 `invalid_token` on `ucp_at_…`**: token is invalid, expired, or revoked. Do not retry as anonymous
- **403 `insufficient_scope`**: buyer token lacks `dev.ucp.shopping.order:read`
- **403 `tier_denied`**: anonymous caller hit checkout or orders
- **validation_error on a line**: wrong customization id (group id instead of option id) or missing required option
- **Checkout `incomplete`**: read `messages` and supply buyer or fulfillment before complete
- **429 `rate_limited`**: token tier has the highest quotas. Back off

## Escalation

- See [Getting started](https://developers.finqu.com/ai/agentic-commerce/getting-started.md.txt)
- See [Prompting](https://developers.finqu.com/ai/agentic-commerce/prompting.md.txt)
- See [Identity linking](https://developers.finqu.com/ai/agentic-commerce/identity-linking.md.txt)
- See [Integration guide](https://developers.finqu.com/ai/agentic-commerce/integration-guide.md.txt)
- See [UCP reference](https://developers.finqu.com/reference/ucp.md.txt)
- See [REST API](https://developers.finqu.com/reference/ucp/rest-api.md.txt)
- See [MCP transport](https://developers.finqu.com/reference/ucp/mcp-transport.md.txt)
- Canonical spec: [ucp.dev](https://ucp.dev)
