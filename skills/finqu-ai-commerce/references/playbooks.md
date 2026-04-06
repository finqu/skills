# Playbooks

Structured step-by-step task sequences for Finqu's AI agent.

## What Are Playbooks?

Playbooks are ordered sequences of instructions that guide the AI through repeatable tasks. They ensure consistency and quality by defining exactly what steps to follow.

## Structure

A playbook consists of:

1. **Name** — Clear, descriptive title
2. **Description** — When and why to use this playbook
3. **Steps** — Ordered list of actions to perform

Each step should be:

- **Specific** — Clear about what to do
- **Measurable** — Has a verifiable outcome
- **Sequential** — Order matters

## Example Playbook

**"Optimize Product Listing"**

```
Step 1: Retrieve current product title, description, and images
Step 2: Analyze title for SEO keywords (max 70 characters)
Step 3: Check description for key product features and benefits
Step 4: Generate improved title with primary keyword
Step 5: Generate improved description with features, benefits, and specifications
Step 6: Suggest image alt text improvements
Step 7: Present all changes for merchant review
Step 8: Apply approved changes
```

## Best Practices

- Keep steps atomic — one action per step
- Include review/approval steps for changes
- Add error handling steps (e.g., "If product not found, ask for correct ID")
- Test playbooks with real store data
- Iterate based on results

## Full Reference

- [Playbooks](https://developers.finqu.com/build-with-finqu/ai-commerce/playbooks.md.txt)
