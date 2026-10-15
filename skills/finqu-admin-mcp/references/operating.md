# Operating Admin MCP

## Session start

1. `tools/list` — schemas and descriptions. An admin tool description includes an `Example output:` sample of `{ "status": "ok", "response": … }`.
2. `get_profile` — user, merchant (`id`, `name`, `hostname`, `currency_id`, `account_status`), and scopes. Unwrapped. No `channel_id`.
3. `list_channels` — `{ merchant_id, channels: [{ id, name, type, store }] }`. Unwrapped. No `channel_id`.
4. Cache the `channel_id` that matches the task. Ask when more than one channel of the required type exists.

## Responses

Successful admin tools:

```json
{ "status": "ok", "response": {} }
```

`get_profile` and `list_channels` are the exceptions. They return the object itself.

Failures set the MCP tool error flag. The text payload is:

```json
{
  "status": "error",
  "response": {
    "message": "Human-readable error",
    "code": "optional_stable_code",
    "suggestions": {}
  }
}
```

`code` and `suggestions` are omitted when unused. Common codes: `release_missing`, `revision_mismatch`, `write_failed`.

Channel validation is plain text, not that envelope:

- Missing `channel_id`: `Validation Error: Missing required parameter 'channel_id'. …`
- Foreign channel: `Access Denied: The specified channel_id (…) does not belong to the authorized merchant account.`
- Wrong channel type: theme tools reject a non-`webstore` channel with a validation error that names the type.

## Channel types

| Tools | Channel `type` |
|-------|----------------|
| Theme, navigation, pages, articles | `webstore` |
| Marketing messages and social | `webstore` or `pos` |
| Catalog and inventory | Any channel the token can use |

## Confirmation

Do not call a destructive or high-impact write until the user confirms. High-impact includes deleting products or variants, bulk price changes, unpublishing, refunds, order cancellation, theme file edits, and inventory adjustments that post stock.

Show the tool name, target id, current value, and proposed value. A read (`search_products`, `get_order`, `get_theme_file`, `list_channels`) does not need that gate.

## Schema rule

Do not copy argument lists from memory or from the grouped catalog page. Call `tools/list` on the connected server and use those schemas.

Grouped catalog (names only): [Admin MCP tools](https://developers.finqu.com/apis-and-tools/mcp/tools.md.txt).

## Full reference

- [Best practices](https://developers.finqu.com/apis-and-tools/mcp/best-practices.md.txt)
- [Admin MCP](https://developers.finqu.com/apis-and-tools/mcp/admin.md.txt)
