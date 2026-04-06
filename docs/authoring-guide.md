# Authoring Guide

How to write and improve Finqu agent skills.

## SKILL.md Template

Every `SKILL.md` follows this structure:

```markdown
---
name: skill-name
description: 'Brief description of what this skill teaches and when to use it'
---

# Skill Title

## When to use

Use this skill when:

- Condition A
- Condition B
- Condition C

## Inputs required

- **Input 1**: Description of what this is and where to find it
- **Input 2**: Description

## Procedure

### 0) Triage / detect

1. First step
2. Second step

Read: `references/topic.md`

### 1) Main task

1. Step with concrete command: `finqu theme dev`
2. Step with link to API docs

Read: `references/another-topic.md`

### 2) Apply changes

1. Implementation step

### 3) Verify

1. Verification step

## Verification

- Expected outcome A
- Expected outcome B
- No errors in logs/console

## Failure modes / debugging

- **Problem X**: Fix by doing Y
- **Problem Z**: Check [docs link](https://developers.finqu.com/...)

## Escalation

- Consult [Finqu Developer Documentation](https://developers.finqu.com)
- Contact Finqu support for platform issues
```

## Writing Principles

### Keep SKILL.md short

The main file should be a procedural checklist — aim for under 200 lines. Move detailed explanations, examples, and deep-dive content into `references/` files.

### Be concrete

Include actual commands, file paths, and code snippets. Avoid vague instructions like "configure the settings" — instead write "edit `config/settings_schema.json` and add a new entry to the `settings` array".

### Link to canonical docs

For API objects, Liquid objects, GraphQL schema, and other reference material, **link to the Finqu Developer Documentation** rather than duplicating:

```markdown
See the [Product object reference](https://developers.finqu.com/reference/liquid/objects/store/product.md.txt)
for all available properties.
```

### Number your steps

Use numbered main steps (0, 1, 2...) in the Procedure section, with sub-steps as needed. Step 0 is typically triage/detection.

### Always include verification

Every procedure must end with concrete verification steps. The AI should know exactly how to confirm the task succeeded.

## Reference Docs

### When to create a reference doc

- The topic needs more than a paragraph of explanation
- Multiple skills might share the same reference
- The content includes detailed examples or patterns

### Reference doc format

Reference docs are plain Markdown files in the `references/` directory. They should:

- Start with a clear heading describing the topic
- Include practical examples
- Link to canonical docs for API/object details
- Be self-contained (readable without SKILL.md context)

### Linking to references

From SKILL.md, reference docs using relative paths:

```markdown
Read: `references/theme-structure.md`
```

## Naming Conventions

- **Skill names**: lowercase, hyphens, prefixed with `finqu-` (e.g., `finqu-liquid-themes`)
- **Reference files**: lowercase, hyphens, descriptive (e.g., `theme-structure.md`, `authentication.md`)
- **Max 64 characters** for skill names

## YAML Frontmatter

Required fields:

| Field         | Description                | Example                                            |
| ------------- | -------------------------- | -------------------------------------------------- |
| `name`        | Unique skill identifier    | `finqu-liquid-themes`                              |
| `description` | Brief one-line description | `"Liquid theme development for Finqu storefronts"` |

The frontmatter is used by the `npx skills` CLI for discovery and listing.
