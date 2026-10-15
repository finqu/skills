---
name: finqu-ai-commerce
description: 'Finqu AI Commerce — Success Agent, Workers, Playbooks, Guidelines, and inbound MCP Tools'
---

# Finqu AI Commerce

## When to use

Use this skill when:

- Configuring the Success Agent in Finqu admin
- Authoring Workers (executor, planner, analyst)
- Authoring Playbooks that merchants run
- Writing Guidelines
- Registering inbound MCP Tools so Finqu AI can call your systems

This skill is **Finqu's own automation** (Work Hub). It is not:

- **Admin MCP** — an external agent calling `https://mcp.finqu.com/mcp`. Use **finqu-admin-mcp**.
- **UCP** — a shopping agent on a storefront. Use **finqu-storefront-mcp**.

Events start Workers and Playbooks. They are a trigger, not the product you are configuring.

## Inputs required

- **Store with AI Commerce / Work Hub**
- **Task**: what the worker or playbook should do
- **Tool whitelist**: exact tool names the worker may call

## Procedure

### 0) Pick the surface

| Goal | Where |
|------|--------|
| External agent operates the store | **finqu-admin-mcp** |
| Finqu AI calls your ERP, CRM, or other MCP server | Inbound MCP Tools (this skill) |
| Background report, plan, or mutation | Worker |
| Ordered steps a merchant runs, or an Event starts | Playbook |
| Shared tone, margin, and brand rules | Guidelines |

Read: `references/success-agent.md`

### 1) Configure a Worker

Roles: `analyst` (read and report), `planner` (propose), `executor` (mutate). Keep write tools off the analyst and planner whitelists.

Required fields: `slug`, `name`, `type`, `system_prompt_extension`, `tool_names`, `status` (`active`, `paused`, or `draft`). Optional: `schedule` (cron), `channel_id`.

Read: `references/workers.md`

### 2) Author a Playbook

A playbook is an ordered `steps` array. Each working step names `worker_type`, `instructions`, and `allowed_tools`. Status is `draft`, `active`, or `archived`.

Merchants run playbooks (manual, bulk, or an Event). The Success Agent can help create a playbook. It does not execute one.

Read: `references/playbooks.md`

### 3) Write Guidelines

Guidelines apply to the Success Agent and to Workers. Put reusable business rules here instead of copying them into every `system_prompt_extension`.

Read: `references/guidelines.md`

### 4) Register inbound MCP Tools

Register the server in Work Hub so Success Agent, Workers, and Playbooks can call it.

- Transport: `streamable_http` or `sse`
- Auth: `bearer`, `oauth2`, or `none`

OAuth sessions on Admin MCP do not receive these tools.

Read: `references/mcp-tools.md`

### 5) Trigger with an Event when the merchant wants automation

An Event binds a store activity or a cron schedule to a Worker or a Playbook. Manual and bulk runs do not need an Event. Webhooks that notify your own app are Admin REST webhooks, not Events.

Read: `references/events.md`

## Verification

- Worker `tool_names` match tools you intend, and analysts have no write tools
- Playbook `status` is `draft` until a merchant should run it
- An active playbook runs from Work Hub (or from an Event), not from a Success Agent chat turn
- Inbound server `tools/list` imports into the merchant tool registry
- Admin MCP `tools/list` on `mcp.finqu.com` does not include those inbound tools

## Failure modes / debugging

- **Agent ignores a guideline**: make the rule specific. More specific guidelines override general ones
- **Playbook step skipped**: give the step an `id`, `instructions`, and an `allowed_tools` list
- **Worker calls the wrong tool**: the call is rejected when the name is absent from `tool_names`
- **Inbound connection fails**: check `url`, `protocol` (`streamable_http` or `sse`), and `auth_type`
- **External agent cannot see a custom tool**: that tool is inbound. Connect the agent with **finqu-admin-mcp** for platform tools only

## Escalation

- See [AI Commerce overview](https://developers.finqu.com/ai/ai-commerce/overview.md.txt)
- See [Workers](https://developers.finqu.com/ai/ai-commerce/workers.md.txt)
- See [Playbooks](https://developers.finqu.com/ai/ai-commerce/playbooks.md.txt)
- See [MCP Tools](https://developers.finqu.com/ai/ai-commerce/mcp-tools.md.txt)
- See [Events](https://developers.finqu.com/ai/ai-commerce/events.md.txt)
- See [Success Agent](https://developers.finqu.com/ai/ai-commerce/success-agent.md.txt)
- See [Guidelines](https://developers.finqu.com/ai/ai-commerce/guidelines.md.txt)
- See [Admin MCP](https://developers.finqu.com/apis-and-tools/mcp/admin.md.txt)
