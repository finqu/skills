# Setup

How to enable UCP and connect an MCP client to a Finqu store.

## Prerequisites

- A Finqu sales channel with a live storefront domain
- UCP enabled on the channel (optionally MCP server)
- A channel API key (`fq_secret_…`) or allowed anonymous/signed access

## Enable UCP (merchants)

In **Channel Settings → API**:

1. Check **Enable UCP**
2. Optionally check **Enable MCP server** for `/mcp` access
3. Configure **Agent access** (anonymous, signed, API keys)
4. Create an API key for production agents
5. Save

Verify:

```bash
curl -s https://{shop-domain}/.well-known/ucp | jq .
curl -s -H "Authorization: Bearer fq_secret_YOUR_KEY" \
  https://{shop-domain}/api/ucp
```

## MCP endpoint

When MCP server is enabled:

```
https://{shop-domain}/mcp
```

## Authentication

```http
Authorization: Bearer fq_secret_…
```

See `references/authentication.md` for signed and anonymous tiers.

## Platform profile (required for MCP tools)

Publish your platform profile at `https://your-platform.example/.well-known/ucp`, then pass it in every MCP tool call:

```json
{
  "meta": {
    "ucp-agent": {
      "profile": "https://your-platform.example/.well-known/ucp"
    }
  }
}
```

## Connecting with Claude Desktop

```json
{
    "mcpServers": {
        "finqu-store": {
            "url": "https://{shop-domain}/mcp",
            "headers": {
                "Authorization": "Bearer fq_secret_YOUR_KEY"
            }
        }
    }
}
```

Note: Claude Desktop may not pass `meta.ucp-agent.profile` automatically. Custom agent frameworks give full control over tool call metadata.

## Connecting programmatically

```typescript
import { Client } from '@modelcontextprotocol/sdk/client/index.js';
import { StreamableHTTPClientTransport } from '@modelcontextprotocol/sdk/client/streamableHttp.js';

const transport = new StreamableHTTPClientTransport(
    new URL('https://{shop-domain}/mcp'),
    {
        requestInit: {
            headers: {
                Authorization: 'Bearer fq_secret_YOUR_KEY',
            },
        },
    },
);

const client = new Client({ name: 'shopping-agent', version: '1.0.0' });
await client.connect(transport);

const { tools } = await client.listTools();
```

When calling tools, include the UCP agent metadata:

```typescript
await client.callTool({
    name: 'search_catalog',
    arguments: {
        query: 'blue shirt',
        limit: 5,
    },
    // Pass via your MCP client's meta/_meta field:
    _meta: {
        'ucp-agent': {
            profile: 'https://your-platform.example/.well-known/ucp',
        },
    },
});
```

## Transport

UCP MCP uses **Streamable HTTP** transport.

## REST alternative

If MCP is not enabled, use the REST API at `https://{shop-domain}/api/ucp` with the same authentication and `UCP-Agent` header. See the [REST API reference](https://developers.finqu.com/reference/ucp/rest-api.md.txt).

## Full Reference

- [MCP overview](https://developers.finqu.com/apis-and-tools/mcp/overview.md.txt)
- [Activating UCP](https://developers.finqu.com/ai/agentic-commerce/activating-ucp.md.txt)
- [MCP Transport](https://developers.finqu.com/reference/ucp/mcp-transport.md.txt)
