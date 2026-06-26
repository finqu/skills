# Available MCP Tools

UCP shopping capabilities exposed as MCP tools at `https://{shop-domain}/mcp`.

**Required on every call:** `meta.ucp-agent.profile` pointing to your platform's `/.well-known/ucp`.

These UCP tool names replace the former Storefront MCP names (`search_products`, `add_cart_lines`, `get_cart_checkout_url`, etc.).

## Catalog tools

### search_catalog

Full-text product search with filters and pagination.

REST equivalent: `POST /catalog/search`

**Parameters:** `query`, `filters`, `limit`, `cursor`, `context` (language, currency, country)

**Returns:** `products` array and `pagination` with `cursor` for next page.

### lookup_catalog

Batch lookup of products or variants by ID.

REST equivalent: `POST /catalog/lookup`

**Parameters:** `ids` (array of product or variant IDs)

**Returns:** Full product objects including variants, pricing, and customization metadata.

### get_product

Get a single product by ID.

REST equivalent: `POST /catalog/product`

**Parameters:** `id` (product ID)

**Returns:** Full product details including `metadata.customizations` on variants.

## Cart tools

### create_cart

Create a cart from line items.

REST equivalent: `POST /carts`

**Parameters:** `line_items` — each with `item.id` (variant ID), `quantity`, and optional `customizations`

**Returns:** Cart with `id`, `line_items`, `totals`, `continue_url`, `messages`

**Recommended:** `meta.idempotency-key` to prevent duplicate carts.

### get_cart

Retrieve a cart by ID.

REST equivalent: `GET /carts/{id}`

**Parameters:** `id` (cart ID)

**Returns:** Cart contents, totals (minor currency units), `continue_url`, `discounts`, `messages`.

### update_cart

Replace all line items on a cart (full update, not incremental add/remove).

REST equivalent: `PUT /carts/{id}`

**Parameters:** `id` (cart ID), `line_items` (complete replacement set)

Replaces the former separate `add_cart_lines`, `update_cart_lines`, and `remove_cart_lines` tools.

### cancel_cart

Cancel a cart (clear all items).

REST equivalent: `POST /carts/{id}/cancel`

**Parameters:** `id` (cart ID)

**Required:** `meta.idempotency-key`

## Checkout handoff

There is no `get_cart_checkout_url` tool. Cart responses include `continue_url` — a buyer-facing URL to complete checkout in the merchant's storefront. The checkout API (`dev.ucp.shopping.checkout`) is not yet available on Finqu.

## Product customizations

Before adding configurable products to a cart:

1. Search or look up the product
2. Read `metadata.customizations` on the chosen variant
3. Include `customizations: [{ "id": "…", "value": "…" }]` on each line item
4. Required customization groups must have at least one selection

## Discount codes

When the `dev.ucp.shopping.discount` capability is negotiated, cart create/update requests may include `discounts.codes`.

## Full Reference

- [MCP Transport](https://developers.finqu.com/reference/ucp/mcp-transport.md.txt)
- [REST API](https://developers.finqu.com/reference/ucp/rest-api.md.txt)
- [MCP OpenRPC schema](https://ucp.dev/2026-04-08/services/shopping/mcp.openrpc.json)
