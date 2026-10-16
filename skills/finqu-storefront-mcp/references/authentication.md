# UCP authentication

Discovery (`/.well-known/ucp`) is public. Shopping calls under `/api/ucp/*` require an agent tier, except the OAuth token and revoke routes.

Resolution order on a single `Authorization` header (`AgentAuthenticator`):

1. `Bearer fq_secret_…` → **token** tier (channel API key). `fq_` keys that are not channel secrets are rejected.
2. `Bearer ucp_at_…` → **user** tier (identity-linking access token).
3. `Signature` and `Signature-Input` → **signed** tier, only if that tier is enabled and neither bearer prefix above was present.
4. No credentials → **anonymous**, only if the merchant allows it.
5. Otherwise `401`.

The two bearers are not combined. Sending `ucp_at_…` does not also apply `fq_secret_…`.

## Tiers and resources

| Resource | Anonymous | Signed | Token (`fq_secret_`) | User (`ucp_at_`) |
|----------|-----------|--------|----------------------|------------------|
| Catalog, cart | Yes | Yes | Yes | Yes (same quotas as token) |
| Checkout | No | Yes | Yes | Yes. `checkout:manage` attaches the customer. Without it, checkout still proceeds as a guest |
| Order | No | Yes | Yes. Channel key can read orders | Only with `dev.ucp.shopping.order:read`, and only orders owned by the linked customer |

Use `ucp_at_…` when the task is the buyer's own order. Do not describe the channel key as rejected. `OrderService` still serves API-key and signed callers.

## Buyer token failures

Invalid, expired, or revoked `ucp_at_…`:

- HTTP `401`
- Challenge: `error="invalid_token"`, realm is the shop origin (not the string `ucp`)
- `resource_metadata="https://{shop-domain}/.well-known/oauth-protected-resource"`
- Body message `code` is `identity_required`
- Not an anonymous fall-through

Valid buyer token missing `dev.ucp.shopping.order:read` on `GET /orders/{id}`:

- HTTP `403`
- Challenge: `error="insufficient_scope"`, `scope="dev.ucp.shopping.order:read"`
- Same `resource_metadata` URL
- Body `code` is `insufficient_scope`

Fetch `resource_metadata` to find the authorization server from the challenge.

## Channel API keys

Created in channel **Settings → API keys**. Prefix `fq_secret_`. Shown once. Scoped to one channel. Highest rate limits among the non-user tiers.

```http
Authorization: Bearer fq_secret_…
```

## Full reference

- [UCP authentication](https://developers.finqu.com/reference/ucp/authentication.md.txt)
- [Identity linking](https://developers.finqu.com/ai/agentic-commerce/identity-linking.md.txt)
- [Activating UCP](https://developers.finqu.com/ai/agentic-commerce/activating-ucp.md.txt)
