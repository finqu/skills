# Identity linking

`dev.ucp.common.identity_linking` connects a store customer to your agent. The shop is the authorization server. Your agent is a **public** OAuth client: authorization code with PKCE `S256` only. `token_endpoint_auth_methods_supported` is `none`. There is no client secret.

- `client_id` is the platform profile URL, for example `https://your-platform.example/.well-known/ucp`. It must be `https` and must resolve to a UCP profile.
- Issuer is `https://{shop-domain}`, the same host as `/.well-known/ucp`.

Checkout does not require this flow. Link when you need the buyer's orders, or when you want checkout attached to the customer.

## Discovery

Both documents are public. They return `404` when UCP or identity linking is off.

```bash
curl -sS "https://{shop-domain}/.well-known/oauth-authorization-server"
curl -sS "https://{shop-domain}/.well-known/oauth-protected-resource"
```

| Field | Value |
|-------|--------|
| `authorization_endpoint` | `https://{shop-domain}/customer/authorize` |
| `token_endpoint` | `https://{shop-domain}/api/ucp/oauth/token` |
| `revocation_endpoint` | `https://{shop-domain}/api/ucp/oauth/revoke` |
| `code_challenge_methods_supported` | `S256` |
| `grant_types_supported` | `authorization_code` and `refresh_token`. A store with an external identity provider may also list `urn:ietf:params:oauth:grant-type:jwt-bearer`. Use the document you fetched |

Protected-resource `resource` and `authorization_servers` are the shop origin. `WWW-Authenticate` challenges point at it with `resource_metadata`.

## Scopes

| Scope | Gate |
|-------|------|
| `dev.ucp.shopping.order:read` | Gated. Required for `GET /api/ucp/orders/{id}` on a buyer token. Orders must belong to the linked customer |
| `dev.ucp.shopping.checkout:manage` | Optional. Attaches the checkout to the customer and can prefill saved details. Without it, checkout still proceeds and the customer is not attached |

## Browser consent

Send the buyer to `GET /customer/authorize` in a browser. The buyer must be signed in. A signed-out `GET` redirects to login and returns. A signed-out `POST` is `403`.

Query: `response_type=code`, `client_id` (profile URL), `redirect_uri` (absolute `https`, or `http` on loopback), at least one scope, `state`, `code_challenge`, `code_challenge_method=S256`.

The consent page posts back to the same path as `application/x-www-form-urlencoded`. Echo the authorize parameters and include the page's `csrf_token`. `decision` is `approve` or `deny`. Any value other than `approve` is a denial.

Approval redirects to `redirect_uri` with `code` (prefix `ucp_ac_`, single use), `state`, and `iss`. Denial redirects with `error=access_denied`.

## Token and revoke

`POST /api/ucp/oauth/token` and `POST /api/ucp/oauth/revoke` take `application/x-www-form-urlencoded`. They do not use the channel API key. Responses are OAuth JSON, not a UCP envelope. `Cache-Control: no-store`.

Exchange: `grant_type=authorization_code`, `code`, the same `redirect_uri`, and `code_verifier` (43–128 characters from `[A-Za-z0-9-._~]`).

```json
{
  "access_token": "ucp_at_…",
  "token_type": "Bearer",
  "expires_in": 3600,
  "refresh_token": "ucp_rt_…",
  "scope": "dev.ucp.shopping.order:read"
}
```

Refresh with `grant_type=refresh_token`. The presented refresh token is consumed. A bad code or refresh token is `400` `invalid_grant`.

Revoke with `token` set to an access or refresh token. Revoking a refresh token invalidates every token for that identity. Unknown tokens still return `200`.

## Order read

```http
GET /api/ucp/orders/{id}
Authorization: Bearer ucp_at_…
```

Do not put `fq_secret_…` on that same request. The channel key can still read orders on its own request. For the buyer's own order, use `ucp_at_…`.

## Full reference

- [Identity linking](https://developers.finqu.com/ai/agentic-commerce/identity-linking.md.txt)
- [REST API — OAuth](https://developers.finqu.com/reference/ucp/rest-api.md.txt)
