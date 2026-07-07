# UCP Fundamentals

Core concepts for integrating AI shopping agents with Finqu via the [Universal Commerce Protocol (UCP)](https://ucp.dev).

**Protocol version:** `2026-04-08`

## Roles

| Role | On Finqu |
|------|----------|
| **Business** | The merchant's sales channel (storefront domain) |
| **Platform** | Your agent, assistant, or integration |

## Discovery profile

Before calling any API, fetch the merchant profile:

```
GET https://{shop-domain}/.well-known/ucp
```

Public, unauthenticated, cacheable. Key fields:

| Field | Purpose |
|-------|---------|
| `ucp.version` | Protocol version — must match your integration |
| `ucp.services` | Available transports and base endpoints |
| `ucp.capabilities` | Shopping capabilities the merchant advertises |

If UCP is not enabled, this endpoint returns `404`.

## Transports

| Transport | Endpoint | When available |
|-----------|----------|----------------|
| **REST** | `https://{shop-domain}/api/ucp` | Always when UCP is enabled |
| **MCP** | `https://{shop-domain}/mcp` | When merchant enables MCP server |
| **Embedded** | `/ucp/host.js` | Hosted checkout iframe (when checkout capability is available) |

## Capabilities

Capabilities use reverse-DNS identifiers. Finqu intersects the merchant's advertised set with your platform profile per request.

### Currently available

| Capability | Description |
|------------|-------------|
| `dev.ucp.shopping.catalog.search` | Full-text product search |
| `dev.ucp.shopping.catalog.lookup` | Batch lookup by ID |
| `dev.ucp.shopping.cart` | Create, read, update, cancel carts |
| `dev.ucp.shopping.discount` | Coupon codes on carts (extends cart) |

### Not yet available

Checkout, fulfillment, order, buyer consent, and identity linking are in the UCP spec but not exposed on Finqu today. Direct buyers to `continue_url` on carts for storefront checkout.

## Capability negotiation

Publish your platform profile at `https://your-platform.example/.well-known/ucp`, then declare it on each request.

**Rules:**

- Protocol versions must match exactly (`2026-04-08`)
- Only capabilities in **both** profiles are active
- Extension capabilities are removed if their parent was not negotiated
- Every response includes a `ucp` block with `version`, `status`, and negotiated `capabilities`

### REST

```http
UCP-Agent: profile="https://your-platform.example/.well-known/ucp"
```

### MCP

```json
{
  "meta": {
    "ucp-agent": {
      "profile": "https://your-platform.example/.well-known/ucp"
    }
  }
}
```

## Responses

Every UCP response includes protocol metadata. Inspect the `ucp` block and `messages` array on every call.

- Monetary amounts are **integers in minor currency units** (cents, öre)
- Cart responses include `continue_url` for buyer checkout handoff
- Use `Idempotency-Key` on cart create and cancel

## Full Reference

- [Agentic Commerce overview](https://developers.finqu.com/ai/agentic-commerce/overview.md.txt)
- [Integration Guide](https://developers.finqu.com/ai/agentic-commerce/integration-guide.md.txt)
- [UCP API Reference](https://developers.finqu.com/reference/ucp.md.txt)
- [Responses & Errors](https://developers.finqu.com/reference/ucp/responses-and-errors.md.txt)
