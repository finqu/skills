---
name: finqu-storefront-api
description: 'Finqu Storefront GraphQL API — queries, mutations, customer authentication, and pagination'
---

# Finqu Storefront GraphQL API

## When to use

Use this skill when:

- Querying product, cart, or store data via the Storefront GraphQL API
- Implementing cart operations (create, add lines, update, remove)
- Setting up customer authentication with access tokens
- Building custom queries or mutations against the Storefront API
- Debugging GraphQL errors or pagination issues

## Inputs required

- **Store domain**: e.g., `your-store.finqu.com`
- **API key**: Storefront API key from Channel settings (prefixed `fq_`)
- **API version**: check the project for which version it uses; the latest stable is `1.3.0` but the project may target a different one

## Procedure

### 0) Understand the API

1. The Storefront API is a **GraphQL** API (not REST)
2. Requires API key authentication via `Authorization` header
3. Supports customer authentication for personalized data
4. Uses cursor-based pagination
5. Rate limits based on query complexity
6. **Always check the project's existing code or configuration to determine which API version it uses** — do not assume the latest

### 1) Authenticate

**API key** (required for all requests):

```http
POST https://your-store.finqu.com/graphql/1.3.0
Authorization: Bearer fq_public_your_api_key
Content-Type: application/json

{
  "query": "{ store { name } }"
}
```

The storefront route is `/graphql/{version}`. It accepts `Authorization: Bearer` plus an `fq_public_…` or `fq_secret_…` key. Use a public key in the browser and a secret key only on a server.

**Customer authentication** (for personalized data):

1. Create customer access token via `customerAccessTokenCreate` mutation
2. Pass the token via the `@storeContext` directive for customer-specific queries
3. Handle token renewal (`customerAccessTokenRenew`) and deletion

Read: `references/authentication.md`

### 2) Use store context

Use the `@storeContext` directive to control language, country, currency, or authenticate as a customer:

```graphql
query @storeContext(language: "fi", country: "FI", currency: "EUR") {
  catalog { products { edges { node { id title price } } } }
}
```

Read: `references/store-context.md`

### 3) Query data

Common queries:

- `product(id: ID!)` — Single product
- `productByHandle(handle: String!)` — Product by URL handle
- `products(...)` — Product listing with filters and sorting
- `productGroup(id: ID!)` — Category/collection
- `productGroups(...)` — Category listing
- `cart(id: ID!)` — Cart contents
- `customer` — Authenticated customer profile (requires `@storeContext` with `customerAccessToken`)
- `order(id: ID!)` — Single order for authenticated customer
- `orders(...)` — Order listing for authenticated customer
- `store` — Store information
- `menus` — Navigation menus
- `countries`, `currencies`, `locales` — Internationalization data

Read: `references/queries.md`

### 4) Mutate data

Common mutations:

- `cartCreate` — Create a new cart
- `cartLinesAdd` — Add items to cart
- `cartLinesUpdate` — Update quantities
- `cartLinesRemove` — Remove items
- `cartDiscountCodesUpdate` — Apply/remove discount codes
- `cartBuyerIdentityUpdate` — Associate customer with cart
- `cartNoteUpdate` — Add order notes
- `customerAccessTokenCreate` — Customer login
- `customerAccessTokenDelete` — Customer logout
- `customerCreate` — Customer registration
- `customerUpdate` — Update customer profile (requires `@storeContext` with `customerAccessToken`)

Read: `references/mutations.md`

### 5) Handle pagination

1. Use cursor-based pagination with `first`/`after` or `last`/`before`
2. Check `pageInfo.hasNextPage` and `pageInfo.endCursor`
3. Request only the fields you need to reduce query complexity

Read: `references/pagination.md`

### 6) Handle errors

1. GraphQL errors appear in the `errors` array of the response
2. Mutation errors appear in `userErrors` of the mutation payload
3. HTTP 429 means rate limit exceeded

Read: `references/error-handling.md`

## Verification

- Queries return expected data structures
- Cart operations complete without `userErrors`
- Customer authentication flow works (create token → use token → renew → delete)
- Pagination correctly traverses all results
- Error handling catches and reports all error types

## Failure modes / debugging

- **Empty response**: Check API key is correct and starts with `fq_`
- **"Unauthorized" error**: API key invalid or not associated with the correct channel
- **userErrors in mutation**: Read the error codes and messages — they describe exactly what's wrong
- **Pagination loop**: Ensure you're advancing the cursor using `pageInfo.endCursor`
- **Rate limited (429)**: Reduce query complexity or request frequency

## Escalation

- See [Storefront API overview](https://developers.finqu.com/apis-and-tools/storefront/overview.md.txt)
- See [Storefront API reference v1.3.0](https://developers.finqu.com/reference/storefront/1.3.0.md.txt)
- See [Storefront API queries](https://developers.finqu.com/reference/storefront/1.3.0/queries/product.md.txt)
- See [Storefront API mutations](https://developers.finqu.com/reference/storefront/1.3.0/mutations/cart-create.md.txt)
