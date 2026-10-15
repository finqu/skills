# Workers

Background agents in Work Hub. Roles are `analyst`, `planner`, and `executor`.

## Roles

| Type | Does | Tools |
|------|------|-------|
| `analyst` | Reports and anomaly summaries | Read-only |
| `planner` | Proposals with concrete parameters | Read-only, including guideline lookup |
| `executor` | Mutations | Write tools you explicitly allow |

An analyst must not hold write tools such as `create_product` or `delete_variant`.

## Configuration

```json
{
  "slug": "weekly-inventory-analyst",
  "name": "Weekly Inventory Analyst",
  "type": "analyst",
  "system_prompt_extension": "Focus on variants below the reorder point. Output a Markdown table.",
  "tool_names": ["get_profile", "list_channels", "list_storages", "search_products"],
  "schedule": "0 6 * * 1",
  "channel_id": 101,
  "status": "active"
}
```

| Field | Required | Notes |
|-------|----------|-------|
| `slug` | Yes | URL-safe id |
| `name` | Yes | Work Hub label |
| `type` | Yes | `analyst`, `planner`, or `executor` |
| `system_prompt_extension` | Yes | Appended to the role prompt |
| `tool_names` | Yes | Whitelist. A call outside this list is denied |
| `schedule` | No | Cron, for example `0 6 * * 1` |
| `channel_id` | No | Default channel injected into the run |
| `status` | Yes | `active`, `paused`, or `draft` |

Names inside `tool_names` must match real tools from `tools/list` (platform tools or imported inbound tools). Do not invent tool names.

## What starts a run

- The `schedule` cron
- An Event (store activity or cron) — see `events.md`
- A Playbook step
- A manual operator action

The runtime injects the role prompt, `system_prompt_extension`, Guidelines, and `channel_id` before the model runs.

## Full reference

- [Workers](https://developers.finqu.com/ai/ai-commerce/workers.md.txt)
