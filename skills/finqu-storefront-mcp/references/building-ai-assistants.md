# Building AI Assistants

How to build custom AI-powered shopping assistants using the Finqu Storefront MCP.

## Architecture

```
User ↔ Your AI App ↔ LLM (with MCP tools) ↔ Finqu Store MCP
```

Your application:

1. Receives user messages
2. Sends them to an LLM with MCP tool definitions
3. LLM decides which tools to call
4. Your app executes tool calls against the store's MCP
5. Returns results to the LLM
6. LLM generates a natural language response

## Implementation Steps

### 1. Set up MCP client

```typescript
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StreamableHTTPClientTransport } from '@modelcontextprotocol/sdk/client/streamableHttp.js';

const transport = new StreamableHTTPClientTransport(new URL('https://your-store.finqu.com/mcp'), {
    requestInit: {
        headers: { Authorization: 'Bearer fq_your_api_key' },
    },
});

const mcpClient = new Client({ name: 'shopping-assistant', version: '1.0.0' });
await mcpClient.connect(transport);
```

### 2. Get available tools

```typescript
const { tools } = await mcpClient.listTools();
// Convert to your LLM's tool format
```

### 3. Handle tool calls

When the LLM requests a tool call:

```typescript
const result = await mcpClient.callTool({
    name: toolName, // e.g., 'search_products'
    arguments: toolArgs, // e.g., { query: 'blue shirt', first: 5 }
});
```

### 4. Shopping flow

A typical conversation:

1. **User**: "I'm looking for a blue shirt"
2. **LLM** calls `search_products({ query: "blue shirt" })`
3. **LLM** presents results: "I found 3 blue shirts..."
4. **User**: "Add the first one to my cart"
5. **LLM** calls `create_cart({ lines: [{ productVariantId: "...", quantity: 1 }] })`
6. **LLM**: "Added to your cart! Would you like to checkout?"
7. **User**: "Yes"
8. **LLM** calls `get_cart_checkout_url({ cartId: "..." })`
9. **LLM**: "Here's your checkout link: ..."

## Best Practices

- Cache the tool list — it doesn't change frequently
- Handle errors gracefully — show friendly messages if a product is out of stock
- Maintain cart ID in session state across conversation turns
- Provide product images and prices in responses for better UX
- Include checkout URL as a clickable link

## Full Reference

- [Storefront MCP overview](https://developers.finqu.com/apis-and-tools/storefront-mcp/overview.md.txt)
- [MCP SDK documentation](https://modelcontextprotocol.io/docs)
