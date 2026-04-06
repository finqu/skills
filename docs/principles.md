# Design Principles

The guiding philosophy behind Finqu agent skills.

## 1. Procedural, not encyclopedic

Skills are checklists that tell agents **what to do**, not textbooks that explain everything. The AI already knows how to code — it needs to know Finqu-specific patterns, pitfalls, and workflows.

## 2. Short SKILL.md, deep references

Keep the main skill file under ~200 lines. Move detailed content into `references/` so the AI can load only what it needs. A single-page checklist is easier for agents to follow than a 1000-line document.

## 3. Upstream docs are canonical

Don't duplicate the Finqu Developer Documentation. Link to `developers.finqu.com` for API objects, Liquid references, GraphQL schema, and other reference material. Author concept and procedure docs manually — these explain the "how" and "why" that raw API docs don't cover.

## 4. Concrete over abstract

Include actual commands (`finqu theme dev`), real file paths (`config/settings_schema.json`), and working code snippets. Avoid vague guidance like "set up authentication" — specify exactly which steps to take.

## 5. Verify everything

Every procedure ends with verification steps. The AI should never be left guessing whether the task succeeded. Include expected outputs, commands to run, and conditions to check.

## 6. One skill, one domain

Each skill covers one coherent area of Finqu development. The router skill handles classification and routing — individual skills handle the actual work. This keeps skills focused and composable.

## 7. Fail gracefully

Include failure modes and debugging steps. When something goes wrong, the AI should know the most likely causes and how to fix them — or when to escalate to human review.

## 8. Target latest stable

Skills target the latest stable versions of Finqu APIs and tools. Document version-specific notes where relevant, but don't maintain parallel instructions for old versions.
