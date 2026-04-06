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
- **API version**: target version (latest stable: `1.0.0`)

## Procedure

### 0) Understand the API

1. The Storefront API is a **GraphQL** API (not REST)
2. Requires API key authentication via `Authorization` header
3. Supports customer authentication for personalized data
4. Uses cursor-based pagination
5. Rate limits based on query complexity

### 1) Authenticate

**API key** (required for all requests):

```http
POST https://your-store.finqu.com/api/storefront
Authorization: Bearer fq_your_api_key
Content-Type: application/json

{
  "query": "{ store { name } }"
}
```

**Customer authentication** (for personalized data):

1. Create customer access token via `customerAccessTokenCreate` mutation
2. Include token in subsequent requests for customer-specific data
3. Handle token renewal (`customerAccessTokenRenew`) and deletion

Read: `references/authentication.md`

### 2) Query data

Common queries:

- `product(id: ID!)` — Single product
- `productByHandle(handle: String!)` — Product by URL handle
- `products(...)` — Product listing with filters and sorting
- `productGroup(id: ID!)` — Category/collection
- `productGroups(...)` — Category listing
- `cart(id: ID!)` — Cart contents
- `store` — Store information
- `menus` — Navigation menus
- `countries`, `currencies`, `locales` — Internationalization data

Read: `references/queries.md`

### 3) Mutate data

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

Read: `references/mutations.md`

### 4) Handle pagination

1. Use cursor-based pagination with `first`/`after` or `last`/`before`
2. Check `pageInfo.hasNextPage` and `pageInfo.endCursor`
3. Request only the fields you need to reduce query complexity

Read: `references/pagination.md`

### 5) Handle errors

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
- See [Storefront API reference v1.0.0](https://developers.finqu.com/reference/storefront/1.0.0.md.txt)
- See [Storefront API queries](https://developers.finqu.com/reference/storefront/1.0.0/queries/product.md.txt)
- See [Storefront API mutations](https://developers.finqu.com/reference/storefront/1.0.0/mutations/cart-create.md.txt)
