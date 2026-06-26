---
name: finqu-storefront-mcp
description: 'Finqu Agentic Commerce — connect AI shopping agents to stores via UCP (REST or MCP transport)'
---

# Finqu Storefront MCP (UCP)

## When to use

Use this skill when:

- Building an AI shopping assistant or agent platform that transacts on Finqu stores
- Connecting an MCP client to a store's `/mcp` endpoint
- Integrating with the [Universal Commerce Protocol (UCP)](https://ucp.dev) on Finqu
- Enabling or verifying UCP on a merchant sales channel
- Choosing between UCP REST (`/api/ucp`) and MCP (`/mcp`) transports

This skill covers **customer-facing agent integrations** (Agentic Commerce). For merchant admin AI (Success Agent, Workers, Playbooks), use **finqu-ai-commerce**.

## Inputs required

- **Store domain**: e.g., `your-store.finqu.com` (sales channel storefront domain)
- **UCP enabled**: merchant has enabled UCP in channel settings (optionally MCP server)
- **Authentication**: channel API key (`fq_secret_…`), signed requests, or anonymous access (if allowed)
- **Platform profile URL**: your `https://your-platform.example/.well-known/ucp` for capability negotiation
- **MCP client** (for MCP transport): Claude Desktop, custom agent, or any MCP-compatible framework

## Procedure

### 0) Understand Agentic Commerce and UCP

Finqu exposes storefront agent access through **UCP**, not a separate Storefront MCP. UCP defines discovery, capabilities, negotiation, and transports (REST, MCP, embedded checkout).

1. Fetch the merchant discovery profile: `GET https://{shop-domain}/.well-known/ucp`
2. Confirm `ucp.version` is `2026-04-08` and inspect advertised capabilities
3. Choose transport: REST (`/api/ucp`) or MCP (`/mcp` when enabled)

Read: `references/ucp-fundamentals.md`

### 1) Enable and verify UCP (merchants)

In channel **Settings → API**:

1. Enable **UCP**
2. Optionally enable **MCP server** for `/mcp` access
3. Configure **Agent access** (anonymous, signed, or API keys only)
4. Create a channel API key for production agents

Verify:

```bash
curl -s https://{shop-domain}/.well-known/ucp | jq .
```

Read: `references/setup.md`

### 2) Configure authentication

Use one of Finqu's three agent tiers:

| Tier | How | Use case |
|------|-----|----------|
| **Token** | `Authorization: Bearer fq_secret_…` | Production agents (highest rate limits) |
| **Signed** | HTTP Message Signatures (RFC 9421) | Verified identity without long-lived secrets |
| **Anonymous** | No credentials (if merchant allows) | Catalog browse and cart only |

Read: `references/authentication.md`

### 3) Publish a platform profile

Publish your platform discovery profile at `https://your-platform.example/.well-known/ucp`, then declare it on every request so Finqu negotiates the capability intersection.

**MCP** — pass in tool call metadata:

```json
{
  "meta": {
    "ucp-agent": {
      "profile": "https://your-platform.example/.well-known/ucp"
    }
  }
}
```

**REST** — send header: `UCP-Agent: profile="https://your-platform.example/.well-known/ucp"`

Read: `references/ucp-fundamentals.md`

### 4) Connect the MCP client

**Endpoint:** `https://{shop-domain}/mcp` (only when MCP server is enabled)

**Authentication:** Bearer token or allowed agent tier (see step 2)

**Transport:** Streamable HTTP

Read: `references/setup.md`

### 5) Use UCP MCP tools

UCP tool names replace the former Storefront MCP names:

| Tool | Purpose |
|------|---------|
| `search_catalog` | Full-text product search with filters and pagination |
| `lookup_catalog` | Batch lookup products or variants by ID |
| `get_product` | Get a single product by ID |
| `create_cart` | Create a cart from line items |
| `get_cart` | Retrieve cart contents and totals |
| `update_cart` | Replace all line items (full cart update) |
| `cancel_cart` | Cancel a cart (requires `meta.idempotency-key`) |

Every tool call requires `meta.ucp-agent.profile`. Cart responses include `continue_url` for storefront checkout (checkout API not yet available).

Read: `references/available-tools.md`

### 6) Build a shopping flow

Typical agent flow:

1. **Discover** — `GET /.well-known/ucp`
2. **Search** — `search_catalog` or `lookup_catalog`
3. **Details** — `get_product`; read `metadata.customizations` for configurable products
4. **Cart** — `create_cart` → `update_cart` as needed
5. **Checkout** — direct buyer to `continue_url` from the cart response

Handle `messages` and `ucp` blocks in every response. Use `Idempotency-Key` on cart create and cancel.

Read: `references/building-ai-assistants.md`

## Verification

- Discovery profile returns `ucp.version`, `ucp.services`, and `ucp.capabilities`
- Authenticated `GET /api/ucp` returns `"protocol": "ucp"` and `"status": "ok"`
- MCP connection lists UCP tools (`search_catalog`, `create_cart`, etc.)
- `search_catalog` returns products with variant IDs and customization metadata
- Cart operations work: create → update → get → cancel
- `continue_url` on cart opens a valid storefront checkout page

## Failure modes / debugging

- **404 on discovery or API**: UCP not enabled on this channel domain — see [Activating UCP](https://developers.finqu.com/ai/agentic-commerce/activating-ucp.md.txt)
- **401 / unauthenticated**: Invalid or missing API key — keys use `fq_secret_` prefix
- **tier_denied**: Auth tier not allowed for this resource (e.g., anonymous on checkout)
- **No MCP tools**: MCP server not enabled — enable in channel settings or use REST transport
- **Empty search results**: Products not published on the storefront channel
- **validation_error on cart**: Missing product customizations — look up product first and include `customizations` array
- **rate_limited (429)**: Back off; token tier has highest quotas

## Escalation

- See [Agentic Commerce overview](https://developers.finqu.com/ai/agentic-commerce/overview.md.txt)
- See [Integration Guide](https://developers.finqu.com/ai/agentic-commerce/integration-guide.md.txt)
- See [UCP API Reference](https://developers.finqu.com/reference/ucp.md.txt)
- See [MCP Transport](https://developers.finqu.com/reference/ucp/mcp-transport.md.txt)
- Canonical spec: [ucp.dev](https://ucp.dev)
