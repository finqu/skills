# Setup

How to connect to a Finqu store's MCP server.

## MCP Endpoint

Every Finqu store with MCP enabled exposes:

```
https://your-store.finqu.com/mcp
```

## Authentication

Use a Channel API key (Storefront API key) from the store's Channel settings:

```
Authorization: Bearer fq_your_api_key
```

## Connecting with Claude Desktop

Add to your Claude Desktop MCP configuration (`claude_desktop_config.json`):

```json
{
    "mcpServers": {
        "finqu-store": {
            "url": "https://your-store.finqu.com/mcp",
            "headers": {
                "Authorization": "Bearer fq_your_api_key"
            }
        }
    }
}
```

## Connecting Programmatically

Use any MCP client library:

```typescript
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StreamableHTTPClientTransport } from '@modelcontextprotocol/sdk/client/streamableHttp.js';

const transport = new StreamableHTTPClientTransport(new URL('https://your-store.finqu.com/mcp'), {
    requestInit: {
        headers: {
            Authorization: 'Bearer fq_your_api_key',
        },
    },
});

const client = new Client({ name: 'my-app', version: '1.0.0' });
await client.connect(transport);

// List available tools
const tools = await client.listTools();
console.log(tools);
```

## Transport

The Storefront MCP uses **Streamable HTTP** transport (the standard MCP transport method).

## Full Reference

- [Storefront MCP overview](https://developers.finqu.com/apis-and-tools/storefront-mcp/overview.md.txt)
