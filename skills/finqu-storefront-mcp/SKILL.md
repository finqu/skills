---
name: finqu-storefront-mcp
description: 'Finqu Storefront MCP — connect AI assistants to stores for product search, cart management, and checkout'
---

# Finqu Storefront MCP

## When to use

Use this skill when:

- Connecting an AI assistant (Claude, ChatGPT, etc.) to a Finqu store via MCP
- Building AI-powered shopping experiences
- Using MCP tools to search products, manage carts, or start checkout
- Setting up an MCP client to communicate with Finqu stores

## Inputs required

- **Store domain**: e.g., `your-store.finqu.com`
- **Channel API key**: Storefront API key from Channel settings (prefixed `fq_`)
- **MCP client**: An AI tool or application that supports MCP (Claude Desktop, custom client, etc.)

## Procedure

### 0) Understand the Storefront MCP

Every Finqu store exposes an MCP server at:

```
https://your-store.finqu.com/mcp
```

This allows AI assistants to interact with the store programmatically — searching products, reading details, creating carts, and generating checkout URLs.

Read: `references/setup.md`

### 1) Configure the MCP connection

Connect your MCP client to the store's MCP endpoint:

**Endpoint:** `https://your-store.finqu.com/mcp`

**Authentication:** Channel API key in the request headers:

```
Authorization: Bearer fq_your_api_key
```

**Transport:** Streamable HTTP (standard MCP transport)

### 2) Use available tools

The Storefront MCP provides these tools:

| Tool                    | Purpose                                      |
| ----------------------- | -------------------------------------------- |
| `search_products`       | Search products by keyword, filter, and sort |
| `get_product`           | Get detailed product information by ID       |
| `create_cart`           | Create a new shopping cart                   |
| `add_cart_lines`        | Add products to a cart                       |
| `update_cart_lines`     | Update quantities in a cart                  |
| `remove_cart_lines`     | Remove items from a cart                     |
| `get_cart`              | Get current cart contents and totals         |
| `get_cart_checkout_url` | Generate a checkout URL for the cart         |

Read: `references/available-tools.md`

### 3) Build a shopping flow

Typical AI-assisted shopping flow:

1. **Search**: Customer describes what they want → `search_products`
2. **Browse**: AI presents matching products → `get_product` for details
3. **Cart**: Customer selects items → `create_cart` + `add_cart_lines`
4. **Checkout**: Ready to buy → `get_cart_checkout_url`
5. **Handoff**: AI provides checkout URL → customer completes purchase in browser

### 4) Build custom AI assistants

Use the MCP endpoint in your own AI applications:

1. Set up an MCP client in your application
2. Connect to the store's MCP endpoint with the API key
3. List available tools
4. Call tools based on user intent
5. Present results in your UI

Read: `references/building-ai-assistants.md`

## Verification

- MCP connection is established (list tools returns available tools)
- `search_products` returns relevant results for test queries
- `get_product` returns complete product details
- Cart operations work: create → add → update → remove → checkout URL
- Checkout URL opens a valid checkout page

## Failure modes / debugging

- **Connection refused**: Check store domain is correct and MCP is enabled
- **401 Unauthorized**: API key invalid — verify it starts with `fq_` and is from Channel settings
- **No tools returned**: MCP may not be enabled for this store — check store settings
- **Empty search results**: Verify products exist and are published on the storefront channel
- **Cart errors**: Check product variant IDs are valid and in stock

## Escalation

- See [Storefront MCP overview](https://developers.finqu.com/apis-and-tools/storefront-mcp/overview.md.txt)
- See [Storefront MCP tools](https://developers.finqu.com/apis-and-tools/storefront-mcp/tools.md.txt)
