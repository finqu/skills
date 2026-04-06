# Nexus Theme

Nexus is Finqu's official Next.js 16-based headless storefront theme with an integrated visual page builder.

## Overview

- **Framework**: Next.js 16 with App Router
- **Visual editing**: Puck drag-and-drop page builder
- **UI components**: shadcn/ui + Radix primitives
- **API integration**: `@finqu/storefront-sdk` (type-safe)
- **Language**: TypeScript

## Key Features

- Dynamic routing — resolves any URL to the correct Finqu resource (product, category, page)
- Visual page builder — customize product, category, and custom pages via drag-and-drop
- Multi-locale support — automatic detection and switching
- Server-side rendering — fast initial page loads and SEO
- Component library — pre-built, customizable components

## Quick Start

```bash
pnpm install
pnpm dev        # Development server
pnpm build      # Production build
pnpm start      # Production server
```

## Project Structure

The Nexus theme follows Next.js App Router conventions:

- `app/` — Routes, layouts, and pages
- `components/` — Reusable React components
- `lib/` — SDK configuration, utilities, helpers
- `blocks/` — Puck visual editor blocks/components
- `public/` — Static assets

## Documentation

- [Architecture](https://developers.finqu.com/build-with-finqu/storefront/nexus-theme/architecture.md.txt)
- [Templates](https://developers.finqu.com/build-with-finqu/storefront/nexus-theme/templates.md.txt)
- [Blocks](https://developers.finqu.com/build-with-finqu/storefront/nexus-theme/blocks.md.txt)
- [Data Fetching](https://developers.finqu.com/build-with-finqu/storefront/nexus-theme/data-fetching.md.txt)
- [Multi-Locale](https://developers.finqu.com/build-with-finqu/storefront/nexus-theme/multi-locale.md.txt)
- [Visual Editing](https://developers.finqu.com/build-with-finqu/storefront/nexus-theme/visual-editing.md.txt)
- [Environment Variables](https://developers.finqu.com/build-with-finqu/storefront/nexus-theme/environment-variables.md.txt)
- [Installation](https://developers.finqu.com/build-with-finqu/storefront/nexus-theme/installation.md.txt)
