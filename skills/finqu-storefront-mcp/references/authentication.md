# Authentication

How agents authenticate to Finqu UCP endpoints.

## Tiers

Finqu supports three authentication tiers. The merchant chooses which are allowed in channel settings.

| Tier | How to authenticate | Typical use |
|------|---------------------|-------------|
| **Token** | `Authorization: Bearer fq_secret_…` | Production agents with full access |
| **Signed** | HTTP Message Signatures ([RFC 9421](https://www.rfc-editor.org/rfc/rfc9421)) | Verified identity without long-lived secrets |
| **Anonymous** | No credentials (if enabled) | Public catalog browse and cart experimentation |

## Tier access matrix

| Resource | Anonymous | Signed | Token |
|----------|-----------|--------|-------|
| Catalog | Yes | Yes | Yes |
| Cart | Yes | Yes | Yes |
| Checkout | No | Yes | Yes |
| Order | No | Yes | Yes |

Anonymous agents can search and manage carts but cannot access checkout or order endpoints.

## API keys

Merchants create keys in **Channel Settings → API keys**. Keys are prefixed `fq_secret_` and shown only once.

```http
Authorization: Bearer fq_secret_…
```

Token tier has the highest rate limits and full access to all enabled UCP resources.

## Signed requests

When signed access is enabled, sign requests with HTTP Message Signatures. Your agent profile URL is extracted from the signature material for capability negotiation.

## Enabling access (merchants)

In channel **Settings → API**:

1. Enable **UCP**
2. Under **Agent access**, enable anonymous and/or signed access as needed
3. Create API keys for production agents

## Full Reference

- [Activating UCP](https://developers.finqu.com/ai/agentic-commerce/activating-ucp.md.txt)
- [UCP Authentication](https://developers.finqu.com/reference/ucp/authentication.md.txt)
