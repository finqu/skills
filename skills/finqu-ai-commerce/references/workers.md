# Workers

Specialized AI roles in Finqu's AI Commerce system.

## Worker Types

### Executor

Carries out direct, concrete tasks:

- Edit product details (title, description, price)
- Update inventory levels
- Generate content (descriptions, meta tags)
- Process data operations

The Executor has access to store modification tools and follows guidelines for how changes should be made.

### Planner

Handles complex, multi-step requests:

- Breaks down complex objectives into ordered steps
- Creates actionable plans with clear milestones
- Coordinates between different store areas

The Planner creates plans that the Executor then carries out step by step.

### Analyst

Provides insights and analysis:

- Sales performance analysis
- Product performance reports
- Customer behavior insights
- Inventory optimization suggestions

The Analyst reads data and provides insights but doesn't modify store data.

## Configuration

Workers are configured with:

- **Available tools** — Which store/external tools they can access
- **Guidelines** — Rules governing their behavior
- **Permissions** — What they can read/modify

## Full Reference

- [Workers](https://developers.finqu.com/build-with-finqu/ai-commerce/workers.md.txt)
