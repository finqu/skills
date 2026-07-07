# Building AI Assistants

How to build custom AI shopping assistants against Finqu stores using UCP.

## Architecture

```
User ↔ Your AI App ↔ LLM (with MCP tools) ↔ UCP MCP at {shop-domain}/mcp
                                              ↕
                                    Finqu store channel
```

Your application:

1. Discovers the merchant via `/.well-known/ucp`
2. Receives user messages and sends them to an LLM with MCP tool definitions
3. LLM decides which UCP tools to call
4. Your app executes tool calls with `meta.ucp-agent.profile` on each call
5. Returns results to the LLM, inspecting `ucp` and `messages` in responses
6. LLM generates a natural language response

## Implementation steps

### 1. Discover the merchant

```bash
curl -s https://{shop-domain}/.well-known/ucp | jq .
```

Confirm `ucp.version`, available transports (REST, MCP), and capabilities.

### 2. Set up MCP client

```typescript
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StreamableHTTPClientTransport } from '@modelcontextprotocol/sdk/client/streamableHttp.js';

const PLATFORM_PROFILE = 'https://your-platform.example/.well-known/ucp';

const transport = new StreamableHTTPClientTransport(
    new URL('https://{shop-domain}/mcp'),
    {
        requestInit: {
            headers: { Authorization: 'Bearer fq_secret_YOUR_KEY' },
        },
    },
);

const mcpClient = new Client({ name: 'shopping-assistant', version: '1.0.0' });
await mcpClient.connect(transport);
```

### 3. Get available tools

```typescript
const { tools } = await mcpClient.listTools();
// UCP tools: search_catalog, lookup_catalog, get_product,
//            create_cart, get_cart, update_cart, cancel_cart
```

### 4. Handle tool calls with UCP metadata

```typescript
const result = await mcpClient.callTool({
    name: 'search_catalog',
    arguments: { query: 'blue shirt', limit: 5 },
    _meta: {
        'ucp-agent': { profile: PLATFORM_PROFILE },
    },
});
```

### 5. Shopping flow

A typical conversation:

1. **User**: "I'm looking for a blue shirt"
2. **LLM** calls `search_catalog({ query: "blue shirt" })`
3. **LLM** presents results with prices and variant IDs
4. **User**: "Add the first one to my cart"
5. **LLM** calls `get_product` if customizations are needed, then `create_cart({ line_items: [{ item: { id: "variant-id" }, quantity: 1 }] })`
6. **LLM**: "Added to your cart! Ready to checkout?"
7. **User**: "Yes"
8. **LLM** reads `continue_url` from the cart response and shares it with the user

Until the checkout API is available, always use `continue_url` for checkout handoff — there is no separate checkout URL tool.

### 6. Handle errors and messages

Inspect every response:

- `ucp.status` — `success` or `error`
- `messages` — warnings, validation errors, rejected coupon codes
- `severity` — `recoverable`, `unrecoverable`, or `requires_buyer_input`

## Best practices

- Fetch and cache the discovery profile — re-check when capabilities change
- Maintain cart ID in session state across conversation turns
- Look up products before adding to cart when customizations exist
- Use `Idempotency-Key` on `create_cart` and `cancel_cart`
- Pass `context` (language, currency) on catalog calls for localized results
- Present product images, prices, and `continue_url` as clickable links
- Gracefully handle capabilities advertised but not yet callable (checkout, orders)
- Prefer token-tier API keys in production for highest rate limits

## Integration checklist

1. Obtain a storefront domain with UCP enabled
2. Fetch `/.well-known/ucp` and confirm capabilities
3. Create a channel API key or configure signed/anonymous access
4. Publish your own `/.well-known/ucp` platform profile
5. Connect MCP client to `/mcp` with authentication
6. Pass `meta.ucp-agent.profile` on every tool call
7. Build cart flows with `continue_url` checkout handoff
8. Handle `messages`, rate limits, and idempotency in your agent loop

## Full Reference

- [Integration Guide](https://developers.finqu.com/ai/agentic-commerce/integration-guide.md.txt)
- [MCP Transport](https://developers.finqu.com/reference/ucp/mcp-transport.md.txt)
- [Responses & Errors](https://developers.finqu.com/reference/ucp/responses-and-errors.md.txt)
- [MCP SDK documentation](https://modelcontextprotocol.io/docs)
