---
name: finqu-router
description: 'Classifies Finqu projects and routes to the appropriate development skill'
---

# Finqu Router

## When to use

Use this skill **first** for any Finqu-related task to determine which specialized skill applies. Use it when:

- You encounter a Finqu project and need to understand what it is
- The user asks to work on something Finqu-related but hasn't specified the area
- You need to pick the right workflow for a task

## Inputs required

- **Project root**: path to the repo or working directory
- **User request**: what the user wants to accomplish

## Procedure

### 0) Detect project type

Check for these indicators in order:

1. **Liquid theme** — Look for `layout/theme.liquid`, `templates/` with `.liquid` files, `config/settings_schema.json`, `sections/` directory
   → Use **finqu-liquid-themes**

2. **Headless storefront (Nexus/Next.js)** — Look for `next.config.*`, `@finqu/storefront-sdk` in `package.json`, `puck` configuration, `app/` directory with React components
   → Use **finqu-storefront**

3. **Finqu app/integration** — Look for `@finqu/app-link` in `package.json`, OAuth/install endpoint code, App Link initialization (`App.create(...)`)
   → Use **finqu-apps**

4. **REST API integration** — Look for HTTP calls to `*.api.myfinqu.com`, API token usage, `Authorization: Bearer` headers targeting Finqu
   → Use **finqu-rest-api**

5. **AI Commerce project** — Look for Worker configurations, Playbook definitions, MCP tool setup, Success Agent customization
   → Use **finqu-ai-commerce**

6. **Storefront MCP** — Look for MCP server configuration pointing to `<domain>/mcp`, AI assistant code using Finqu storefront tools
   → Use **finqu-storefront-mcp**

7. **Customer communications** — Look for MJML/HTML email templates with Finqu Liquid variables (`{{ customer.* }}`, `{{ order.* }}`), print templates
   → Use **finqu-customer-comms**

### 1) Route by task type (if no project detected)

If no project files are found, route based on what the user wants to do:

| User wants to...                                | Skill                    |
| ----------------------------------------------- | ------------------------ |
| Build or customize a theme                      | **finqu-liquid-themes**  |
| Build a headless storefront                     | **finqu-storefront**     |
| Build a partner app                             | **finqu-apps**           |
| Integrate via REST API                          | **finqu-rest-api**       |
| Query product/cart data via GraphQL             | **finqu-storefront-api** |
| Use the Finqu CLI                               | **finqu-cli**            |
| Set up AI agents, Workers, or Playbooks         | **finqu-ai-commerce**    |
| Build an AI shopping assistant                  | **finqu-storefront-mcp** |
| Customize emails, prints, or marketing messages | **finqu-customer-comms** |

### 2) Cross-cutting concerns

Some tasks span multiple skills:

- **GraphQL API usage** in a storefront → primary: **finqu-storefront**, also read **finqu-storefront-api**
- **CLI commands** in theme development → primary: **finqu-liquid-themes**, also read **finqu-cli**
- **MCP tools** in AI Commerce → primary: **finqu-ai-commerce**, also read **finqu-storefront-mcp**

## Verification

- The correct skill is identified and loaded
- The project type matches file indicators found
- Cross-cutting skills are loaded when tasks span multiple domains

## Failure modes / debugging

- **No indicators found**: Ask the user what type of Finqu project they're working on
- **Multiple project types in one repo**: Route to the skill matching the user's current task, note the others
- **Unknown project structure**: Default to **finqu-cli** for general Finqu operations

## Escalation

- If the project type is unclear after detection, ask the user
- Consult [Finqu Developer Documentation](https://developers.finqu.com) for platform overview
