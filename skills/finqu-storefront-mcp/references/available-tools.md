# Available Tools

MCP tools provided by the Finqu Storefront MCP server.

## Product Tools

### search_products

Search for products by keyword with filtering and sorting.

**Parameters:**

| Parameter | Type    | Description                                 |
| --------- | ------- | ------------------------------------------- |
| `query`   | string  | Search keyword                              |
| `first`   | number  | Number of results (default: 10)             |
| `sortKey` | string  | Sort field (e.g., TITLE, PRICE, CREATED_AT) |
| `reverse` | boolean | Reverse sort order                          |

**Returns:** List of matching products with basic details (ID, title, handle, price, image URL).

### get_product

Get detailed information about a specific product.

**Parameters:**

| Parameter | Type   | Description |
| --------- | ------ | ----------- |
| `id`      | string | Product ID  |

**Returns:** Full product details including title, description, variants, images, pricing, availability.

## Cart Tools

### create_cart

Create a new shopping cart, optionally with initial items.

**Parameters:**

| Parameter | Type  | Description                                         |
| --------- | ----- | --------------------------------------------------- |
| `lines`   | array | Optional initial cart lines (variant ID + quantity) |

**Returns:** Cart ID for use in subsequent operations.

### add_cart_lines

Add products to an existing cart.

**Parameters:**

| Parameter | Type   | Description                          |
| --------- | ------ | ------------------------------------ |
| `cartId`  | string | Cart ID                              |
| `lines`   | array  | Items to add (variant ID + quantity) |

### update_cart_lines

Update quantities or properties of cart items.

**Parameters:**

| Parameter | Type   | Description                              |
| --------- | ------ | ---------------------------------------- |
| `cartId`  | string | Cart ID                                  |
| `lines`   | array  | Lines to update (line ID + new quantity) |

### remove_cart_lines

Remove items from a cart.

**Parameters:**

| Parameter | Type   | Description            |
| --------- | ------ | ---------------------- |
| `cartId`  | string | Cart ID                |
| `lineIds` | array  | IDs of lines to remove |

### get_cart

Get current cart contents and totals.

**Parameters:**

| Parameter | Type   | Description |
| --------- | ------ | ----------- |
| `cartId`  | string | Cart ID     |

**Returns:** Cart contents, line items, quantities, prices, estimated totals.

### get_cart_checkout_url

Generate a checkout URL for the cart.

**Parameters:**

| Parameter | Type   | Description |
| --------- | ------ | ----------- |
| `cartId`  | string | Cart ID     |

**Returns:** URL that the customer can open in a browser to complete checkout.

## Full Reference

- [Storefront MCP tools](https://developers.finqu.com/apis-and-tools/storefront-mcp/tools.md.txt)
