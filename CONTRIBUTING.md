# Contributing to Finqu Agent Skills

Thanks for your interest in improving Finqu skills for AI assistants! Most skills are written in Markdown — you don't need to be a coding wizard to contribute.

## Ways to Contribute

- **Improve existing skills** — Fix outdated patterns, add missing steps, improve verification procedures
- **Add reference docs** — Expand coverage of specific topics in a skill's `references/` directory
- **Create new skills** — Cover new Finqu development areas
- **Report issues** — Found an AI assistant following wrong advice? Open an issue

## Creating a New Skill

### 1. Scaffold the skill

```bash
npx skills init my-skill-name
```

Or manually create the directory structure:

```
skills/my-skill-name/
├── SKILL.md
└── references/
    └── topic.md
```

### 2. Write the SKILL.md

Every `SKILL.md` must include YAML frontmatter and follow the standard sections. See the [Authoring Guide](docs/authoring-guide.md) for the full template.

**Required YAML frontmatter:**

```yaml
---
name: my-skill-name
description: 'One-line description of what this skill teaches'
---
```

**Required sections:**

1. `## When to use` — Conditions that trigger this skill
2. `## Inputs required` — What the AI needs before proceeding
3. `## Procedure` — Step-by-step checklist
4. `## Verification` — How to confirm the task worked
5. `## Failure modes / debugging` — Common problems and fixes
6. `## Escalation` — When to consult docs or humans

### 3. Add reference docs

Move detailed explanations into `references/` files. Keep `SKILL.md` short and procedural — it should read like a checklist, not a textbook.

For API objects, Liquid references, and GraphQL schema, **link to the canonical docs** at `developers.finqu.com` rather than duplicating content. Manually author concept and procedure docs.

### 4. Test your skill

- Install locally: `npx skills add ./ --skill my-skill-name`
- Verify discovery: `npx skills add ./ --list`
- Test with an AI assistant — give it a task that should trigger the skill and verify it follows the procedure

## Style Guidelines

- **Procedural, not explanatory** — Skills are checklists, not tutorials
- **Short SKILL.md** — Keep under ~200 lines; move depth into `references/`
- **Concrete commands** — Include actual commands to run, not abstract descriptions
- **Link to upstream** — Reference `developers.finqu.com` for API details, don't duplicate
- **Verify everything** — Every procedure must end with verification steps

## Pull Request Process

1. Fork and branch from `main`
2. Make your changes
3. Ensure `npx skills add ./ --list` discovers your skill
4. Verify YAML frontmatter is valid (`name` and `description` required)
5. Open a PR with a clear description of what changed and why

## Code of Conduct

Be respectful and constructive. We're building tools to help developers — keep that spirit in your contributions.
