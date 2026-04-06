---
name: finqu-ai-commerce
description: 'Finqu AI Commerce — Success Agent, Workers, Playbooks, Guidelines, and MCP Tools'
---

# Finqu AI Commerce

## When to use

Use this skill when:

- Configuring Finqu's AI agent (Success Agent) for a store
- Building or modifying Workers (Executor, Planner, Analyst)
- Creating Playbooks (step-by-step task sequences for the AI)
- Writing Guidelines (rules and context for AI behavior)
- Connecting external MCP Tools to the AI agent

## Inputs required

- **Store with AI Commerce enabled**
- **Understanding of the task to automate**: What the AI agent should do
- **Relevant context**: Product info, policies, tone of voice

## Procedure

### 0) Understand Success Agent

The Success Agent is Finqu's built-in AI agent. It operates in three modes:

1. **Ask** — Answer questions using store knowledge
2. **Agent** — Execute tasks (modify products, create content, etc.)
3. **Planning** — Create multi-step plans for complex tasks

The agent uses Workers, Playbooks, and Guidelines to know what to do and how.

Read: `references/success-agent.md`

### 1) Configure Workers

Workers are specialized AI roles:

- **Executor** — Carries out direct tasks (edit product, update inventory)
- **Planner** — Breaks complex requests into step-by-step plans
- **Analyst** — Analyzes data and provides insights

Each worker has access to specific tools and follows configured guidelines.

Read: `references/workers.md`

### 2) Create Playbooks

Playbooks are ordered sequences of steps that guide the AI through complex tasks:

```
Playbook: "Optimize product listing"
Step 1: Analyze current product title and description
Step 2: Research competitor listings
Step 3: Generate improved title (max 70 chars)
Step 4: Generate improved description with key features
Step 5: Review and present changes for approval
```

Playbooks ensure consistency and quality for repeatable tasks.

Read: `references/playbooks.md`

### 3) Write Guidelines

Guidelines provide context and rules that shape AI behavior:

- **Tone of voice** — How the AI should communicate
- **Brand rules** — Product naming conventions, prohibited terms
- **Business rules** — Pricing policies, shipping constraints
- **Domain knowledge** — Product specifications, industry terms

Read: `references/guidelines.md`

### 4) Connect MCP Tools

MCP (Model Context Protocol) tools extend the AI agent's capabilities by connecting external services:

**Authentication methods:**

- **OAuth** — For services requiring user authorization
- **Bearer token** — For API key-based services

**Transport methods:**

- **SSE** (Server-Sent Events) — Streaming connections
- **Streamable HTTP** — Standard HTTP with streaming support

Read: `references/mcp-tools.md`

### 5) Test the configuration

1. Open the AI agent in the store admin
2. Try each mode (Ask, Agent, Planning)
3. Test playbooks by requesting the automated tasks
4. Verify guidelines are followed in responses
5. Confirm MCP tools connect and function correctly

## Verification

- Success Agent responds in the configured tone of voice
- Workers execute their designated tasks correctly
- Playbooks produce consistent, repeatable results
- Guidelines are respected (no violations of brand/business rules)
- MCP Tools connect successfully and return expected data

## Failure modes / debugging

- **Agent doesn't follow guidelines**: Check guideline priority and specificity — more specific guidelines override general ones
- **Playbook steps skipped**: Ensure steps are clearly ordered and have explicit completion criteria
- **MCP Tool connection fails**: Verify auth credentials, check endpoint URL, confirm transport method
- **Agent hallucinates**: Add more specific guidelines with examples, reduce ambiguity
- **Worker uses wrong tool**: Review worker tool assignments in configuration

## Escalation

- See [AI Commerce overview](https://developers.finqu.com/build-with-finqu/ai-commerce/overview.md.txt)
- See [Success Agent](https://developers.finqu.com/build-with-finqu/ai-commerce/success-agent.md.txt)
- See [Workers](https://developers.finqu.com/build-with-finqu/ai-commerce/workers.md.txt)
- See [Playbooks](https://developers.finqu.com/build-with-finqu/ai-commerce/playbooks.md.txt)
- See [MCP Tools](https://developers.finqu.com/build-with-finqu/ai-commerce/mcp-tools.md.txt)
