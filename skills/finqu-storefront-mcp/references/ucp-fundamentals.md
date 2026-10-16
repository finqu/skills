# UCP fundamentals

**Protocol version:** `2026-08-25`

## Discovery

```
GET https://{shop-domain}/.well-known/ucp
```

Public. `404` when UCP is off for that domain.

| Field | Purpose |
|-------|---------|
| `ucp.version` | Must be `2026-08-25` for this release |
| `ucp.services` | REST and MCP bindings under `dev.ucp.shopping` and `dev.ucp.common` |
| `ucp.capabilities` | What this store advertises |
| `ucp.payment_handlers` | Includes `com.finqu.hosted_checkout` |
| `keys` | Public signing keys |

Transports: REST at `https://{shop-domain}/api/ucp` whenever UCP is enabled. MCP at `https://{shop-domain}/mcp` when the merchant enables the MCP server. Embedded checkout is a separate transport (`/ucp/host.js`).

## Capabilities the code advertises

When UCP is enabled, the business profile always includes:

| Capability | Role |
|------------|------|
| `dev.ucp.shopping.catalog.search` | Search |
| `dev.ucp.shopping.catalog.lookup` | Lookup by id |
| `dev.ucp.shopping.cart` | Carts |
| `dev.ucp.shopping.checkout` | Checkout sessions |
| `dev.ucp.shopping.fulfillment` | Shipping or pickup (`method_combinations`) |
| `dev.ucp.shopping.discount` | Extends cart and checkout |
| `dev.ucp.shopping.order` | Order reads |
| `dev.ucp.common.identity_linking` | Buyer OAuth. Gated scope is `dev.ucp.shopping.order:read` |
| `dev.ucp.shopping.buyer_consent` | Extends cart and checkout |
| `dev.ucp.shopping.permalink` | Purchase links at `https://{shop-domain}/buy` |
| `dev.ucp.common.payment.split_payments` | One gift card with a redirect, or a gift card alone |

Advertised only when the channel can honor them:

| Capability | When |
|------------|------|
| `dev.ucp.common.location.search` and `dev.ucp.common.location.lookup` | Channel has a pickup shipping method |
| `dev.ucp.common.loyalty` | Channel has a lifetime loyalty campaign |
| `dev.ucp.common.payment.terms` | Channel has an invoice payment method |

Read the live profile. Do not call a capability it does not list. Negotiation drops extensions whose parent was not negotiated. If you omit a platform profile, the merchant's advertised set is used.

`com.finqu.hosted_checkout` advertises instruments `redirect` and `gift_card`. Its `checkout_url` is the store checkout host, not an initialized payment session.

## Negotiation

Publish `https://your-platform.example/.well-known/ucp`. Protocol versions must match exactly.

REST:

```http
UCP-Agent: profile="https://your-platform.example/.well-known/ucp"
```

MCP `meta`:

```json
{
  "ucp-agent": {
    "profile": "https://your-platform.example/.well-known/ucp"
  }
}
```

## Money and messages

Amounts are integers in minor units. A `discount` total is negative. Inspect `ucp` and `messages` on every response. `severity` is `recoverable`, `unrecoverable`, or `requires_buyer_input`.

## Full reference

- [Agentic Commerce overview](https://developers.finqu.com/ai/agentic-commerce/overview.md.txt)
- [Integration guide](https://developers.finqu.com/ai/agentic-commerce/integration-guide.md.txt)
- [UCP reference](https://developers.finqu.com/reference/ucp.md.txt)
- [Responses and errors](https://developers.finqu.com/reference/ucp/responses-and-errors.md.txt)
