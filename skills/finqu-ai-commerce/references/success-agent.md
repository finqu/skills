# Success Agent

Merchant chat assistant in Finqu admin. Modes:

1. **Ask** — read and answer. No mutations.
2. **Agent** — call tools and Workers inside the granted permissions.
3. **Planning** — break a request into steps, then execute or present the plan.

The agent can delegate to predefined subagents and to merchant Workers (executor, planner, analyst). It can create or refine a Playbook. It cannot run a Playbook. Merchants run Playbooks, or an Event starts an active one.

Custom skills are saved prompts merchants can invoke by name. Guidelines set tone and guardrails. Inbound MCP Tools extend what the chat can call. Those inbound tools are not part of an external Admin MCP session.

## Full reference

- [Success Agent](https://developers.finqu.com/ai/ai-commerce/success-agent.md.txt)
- [AI Commerce overview](https://developers.finqu.com/ai/ai-commerce/overview.md.txt)
