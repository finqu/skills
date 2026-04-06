# Agent Skills for Finqu

Teach AI coding assistants how to build with Finqu the right way.

Agent Skills are portable bundles of instructions, checklists, and reference docs that help AI assistants (Claude, Copilot, Codex, Cursor, and [40+ more](https://skills.sh/docs/cli#supported-agents)) understand Finqu development patterns, avoid common mistakes, and follow best practices.

> **What is Finqu?** Finqu is a unified commerce platform that connects online and offline sales channels — online stores, POS, social commerce — with powerful APIs, Liquid theming, headless storefronts, and AI commerce capabilities. [Learn more →](https://developers.finqu.com)

## Available Skills

| Skill                    | Description                                                                         |
| ------------------------ | ----------------------------------------------------------------------------------- |
| **finqu-router**         | Classifies Finqu projects and routes to the appropriate skill                       |
| **finqu-liquid-themes**  | Liquid theme development — templates, sections, blocks, settings, layouts, checkout |
| **finqu-storefront**     | Headless storefronts with Next.js, Nexus theme, Storefront SDK, Puck visual editor  |
| **finqu-rest-api**       | REST API integration — authentication, CRUD, error handling, webhooks               |
| **finqu-storefront-api** | Storefront GraphQL API — queries, mutations, customer auth, pagination              |
| **finqu-apps**           | Partner apps & integrations — App Link, OAuth, embedding in admin                   |
| **finqu-cli**            | Finqu CLI — theme dev/deploy, storefront create, configuration                      |
| **finqu-ai-commerce**    | AI Commerce — Success Agent, Workers, Playbooks, Guidelines, MCP Tools              |
| **finqu-customer-comms** | Customer communications — emails, prints, marketing messages with MJML/Liquid       |
| **finqu-storefront-mcp** | Storefront MCP server for AI shopping assistants                                    |

## Quick Start

### Install all skills

```bash
npx skills add Finqu/skills
```

### Install specific skills

```bash
npx skills add Finqu/skills --skill finqu-liquid-themes
npx skills add Finqu/skills --skill finqu-storefront --skill finqu-rest-api
```

### Install globally (available across all projects)

```bash
npx skills add Finqu/skills -g
```

### List available skills

```bash
npx skills add Finqu/skills --list
```

### Install to specific agents

```bash
npx skills add Finqu/skills -a claude-code -a cursor -a github-copilot
```

The CLI automatically detects which coding agents you have installed and installs skills to the appropriate directories.

## How It Works

Each skill contains:

```
skills/finqu-liquid-themes/
├── SKILL.md              # Main instructions (when to use, procedure, verification)
└── references/           # Deep-dive docs on specific topics
    ├── theme-structure.md
    ├── sections.md
    ├── blocks.md
    └── ...
```

When you ask your AI assistant to work on Finqu code, it reads these skills and follows the documented procedures rather than guessing. Reference docs provide detailed knowledge, while `SKILL.md` links to the canonical [Finqu Developer Documentation](https://developers.finqu.com) for API objects, Liquid references, and GraphQL schema.

## Manual Installation

Copy any skill folder from `skills/` into your project's instructions directory for your AI assistant:

| Agent          | Project path      | Global path          |
| -------------- | ----------------- | -------------------- |
| Claude Code    | `.claude/skills/` | `~/.claude/skills/`  |
| GitHub Copilot | `.agents/skills/` | `~/.copilot/skills/` |
| Cursor         | `.agents/skills/` | `~/.cursor/skills/`  |
| Codex          | `.agents/skills/` | `~/.codex/skills/`   |

See the [full agent list](https://skills.sh/docs/cli#supported-agents) for all supported agents.

## Documentation

- [Authoring Guide](docs/authoring-guide.md) — How to create and improve skills
- [Principles](docs/principles.md) — Design philosophy
- [Finqu Developer Docs](https://developers.finqu.com) — Canonical platform documentation

## Contributing

We welcome contributions! Most skills are written in Markdown, focusing on clear procedures and best practices.

See [CONTRIBUTING.md](CONTRIBUTING.md) for details.

```bash
# Scaffold a new skill
npx skills init my-new-skill
```

## License

MIT — see [LICENSE](LICENSE).
