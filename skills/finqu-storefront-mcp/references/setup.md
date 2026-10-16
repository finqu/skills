# Setup

How to enable UCP and connect an MCP client to a Finqu store.

## Prerequisites

- A Finqu sales channel with a live storefront domain
- UCP enabled (MCP server enabled when you use `/mcp`)
- A channel API key (`fq_secret_…`), or signed/anonymous access if the merchant allows it
- For buyer order reads, a `ucp_at_…` token from identity linking

## Enable UCP (merchants)

In **Channel Settings → API** (`/channel/{channelId}/settings`):

1. Check **Enable UCP**
2. Optionally check **Enable MCP server**
3. Under **Agent access**, allow anonymous and/or signed access as needed. Checkout and orders stay closed to anonymous callers
4. Create an API key. The secret is shown once
5. Save

Verify:

```bash
curl -sS "https://{shop-domain}/.well-known/ucp"
curl -sS -H "Authorization: Bearer fq_secret_YOUR_KEY" \
  "https://{shop-domain}/api/ucp"
```

Expect `ucp.version` `2026-08-25` on discovery, and `"protocol": "ucp"` with `"status": "ok"` on the service root.

## MCP client

Endpoint: `https://{shop-domain}/mcp`. Transport: Streamable HTTP.

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

Pass `meta.ucp-agent.profile` on every tool call. A static client config does not add that metadata for you. Swap the bearer to `ucp_at_…` for a buyer order read. Do not send both tokens.

```typescript
await client.callTool({
  name: 'search_catalog',
  arguments: { query: 'linen shirt', context: { language: 'en', currency: 'EUR' } },
  _meta: {
    'ucp-agent': { profile: 'https://your-platform.example/.well-known/ucp' },
  },
});
```

## REST

REST is available whenever UCP is enabled, including when MCP is off.

```http
UCP-Agent: profile="https://your-platform.example/.well-known/ucp"
Authorization: Bearer fq_secret_…
```

## Full reference

- [Activating UCP](https://developers.finqu.com/ai/agentic-commerce/activating-ucp.md.txt)
- [Getting started](https://developers.finqu.com/ai/agentic-commerce/getting-started.md.txt)
- [MCP transport](https://developers.finqu.com/reference/ucp/mcp-transport.md.txt)
