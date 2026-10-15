# Storefront Commands

CLI commands for headless storefront development.

## Create

```bash
finqu storefront create
```

Scaffolds a new headless storefront project. Interactive prompts guide you through:

- Project name
- Template selection (e.g., Nexus theme)
- Store connection
- Configuration

The result is a ready-to-develop Next.js project.

## Development

After creation:

```bash
cd your-storefront
npm install
finqu storefront dev
```

`finqu storefront dev` scans components and writes generated config under `.storefront`. Production is `finqu storefront build`.

## Full Reference

- [Headless storefronts](https://developers.finqu.com/build-with-finqu/storefront/overview.md.txt)
- [Storefront commands](https://developers.finqu.com/apis-and-tools/cli/storefront.md.txt)
