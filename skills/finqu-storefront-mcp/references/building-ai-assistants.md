# Building a shopping agent

One store. Protocol `2026-08-25`. There is no global product index.

## Intent

| Buyer intent | Call |
|--------------|------|
| Products, a category, or a comparison | `POST /api/ucp/catalog/search` (`search_catalog`) immediately |
| One product | `POST /api/ucp/catalog/product` (`get_product`) |
| Bag | `POST /api/ucp/carts` or `PUT /api/ucp/carts/{id}` (`create_cart`, `update_cart`) |
| Ready to buy | `POST /api/ucp/checkout-sessions` with `cart_id` (`create_checkout`) |
| Contact, address, shipping, or a code | `PUT /api/ucp/checkout-sessions/{id}` (`update_checkout`). Copy ids from the session. Restate `line_items` |
| Agrees to pay | `POST …/complete` only when `status` is `ready_for_complete` (`complete_checkout`) |
| Order status | `GET /api/ucp/orders/{id}` with `Bearer ucp_at_…` (`get_order`) |

## Rules

- Search on the first shopping request. Do not ask a clarifying question before that search unless there is no product intent.
- A cart does not need an address. Checkout is the commitment.
- `line_items[].item.id` is the variant id. `customizations[].id` is the option id.
- Do not invent fulfillment ids, shipping option ids, or payment fields. Read them from the checkout session.
- If `status` is `requires_escalation` or `messages` include `payment_escalation`, send `continue_url`. That is the `com.finqu.hosted_checkout` handoff. Do not collect a card. A zero balance can complete without the handoff. `continue_url` can appear on the checkout before that.
- `identity_optional` means guest checkout is allowed.
- Do not invent returns, exchanges, cancellation of placed orders, or reorder actions.
- Read `messages` after every call.
- Money is minor units. Pass `context.language` and `context.currency`.

## MCP client sketch

```typescript
const transport = new StreamableHTTPClientTransport(
  new URL('https://{shop-domain}/mcp'),
  { requestInit: { headers: { Authorization: 'Bearer fq_secret_YOUR_KEY' } } },
);
```

Use a separate client or request header with `Bearer ucp_at_…` for `get_order`. Never put both secrets on one header.

`tools/list` should include `search_catalog`, `lookup_catalog`, `get_product`, `create_cart`, `get_cart`, `update_cart`, `cancel_cart`, `create_checkout`, `get_checkout`, `update_checkout`, `complete_checkout`, `cancel_checkout`, `get_order`, `search_locations`, `lookup_locations`, and `ucp_describe`.

## Checklist

1. Fetch `/.well-known/ucp` and confirm `2026-08-25`
2. Publish your platform profile and send it on every call
3. Create a cart with variant ids and option-level customizations
4. Create a checkout from `cart_id`
5. Complete, and hand off `continue_url` only when a balance remains
6. Link the buyer before reading their order

## Full reference

- [Prompting](https://developers.finqu.com/ai/agentic-commerce/prompting.md.txt)
- [Getting started](https://developers.finqu.com/ai/agentic-commerce/getting-started.md.txt)
- [Integration guide](https://developers.finqu.com/ai/agentic-commerce/integration-guide.md.txt)
- [Responses and errors](https://developers.finqu.com/reference/ucp/responses-and-errors.md.txt)
