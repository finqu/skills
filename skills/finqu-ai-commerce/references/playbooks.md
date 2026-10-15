# Playbooks

Ordered workflows in Work Hub. Merchants run them. The Success Agent can help define or save a playbook. It cannot execute one.

## Status

| Status | Who can run it |
|--------|----------------|
| `draft` | Staff with developer permissions, manual only. No Events |
| `active` | Manual, bulk actions, and Events |
| `archived` | Nobody. Kept for audit history |

## Shape

```json
{
  "slug": "automated-stock-replenishment",
  "name": "Automated Stock Replenishment",
  "description": "Find low stock, plan a transfer, wait for approval, then write it.",
  "status": "draft",
  "triggers": [{ "type": "manual" }],
  "steps": [
    {
      "id": "analyze_stock",
      "name": "Analyze stock",
      "worker_type": "analyst",
      "instructions": "List variants with under 7 days of supply.",
      "allowed_tools": ["list_storages", "search_products"],
      "output_variable": "low_stock_items"
    },
    {
      "id": "review_gate",
      "name": "Merchant confirmation",
      "require_approval": true,
      "instructions": "Review quantities before any write."
    },
    {
      "id": "execute_transfers",
      "name": "Create transfers",
      "worker_type": "executor",
      "instructions": "Create draft transfers from {{steps.analyze_stock.low_stock_items}}.",
      "allowed_tools": ["create_inventory_transfer"],
      "output_variable": "transfer_results"
    }
  ]
}
```

Working steps need `worker_type` (`analyst`, `planner`, or `executor`), `instructions`, and usually `allowed_tools`. Approval gates set `require_approval` and omit `worker_type`.

Later steps read earlier output with `{{steps.<step_id>.<output_variable>}}`. Bulk runs receive `{{context.selected_ids}}`.

`allowed_tools` narrows what that step may call. Keep the names inside the worker's own `tool_names` and inside tools the store actually has.

## How a merchant runs one

1. **Manual** — Work Hub, Playbooks.
2. **Bulk** — select rows in admin and run the playbook.
3. **Event** — a configured Event starts an `active` playbook. See `events.md`.

Publish to `active` only after a manual test. Use `require_approval` before refunds, deletes, or live price changes. The run pauses at `waiting_for_approval` until a person approves or rejects it.

## Full reference

- [Playbooks](https://developers.finqu.com/ai/ai-commerce/playbooks.md.txt)
