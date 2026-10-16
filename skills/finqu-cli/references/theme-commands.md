# Theme Commands

CLI commands for Liquid theme development.

## Configure

```bash
finqu theme configure
```

Connects your local directory to a store theme. Sign in first with `finqu sign-in`. The command stores the store and theme in `.env`.

## Develop

```bash
finqu theme dev
```

Starts a local development environment:

- Watches for file changes in your theme directory
- Syncs changes to the connected store in real-time
- Provides a preview URL for testing
- Hot-reloads on changes

**Common flags:**

| Flag     | Description                    |
| -------- | ------------------------------ |
| `--port` | Custom port for the dev server |
| `--open` | Auto-open browser              |

## Deploy

```bash
finqu theme deploy
```

Uploads the current theme to the connected store. Replaces the theme files on the server with your local version.

## Workflow

Typical theme development cycle:

```bash
# 1. Set up connection
finqu theme configure

# 2. Start local dev
finqu theme dev

# 3. Make changes, preview in browser

# 4. When ready, deploy
finqu theme deploy
```

## Full Reference

- [Theme commands](https://developers.finqu.com/apis-and-tools/cli/theme.md.txt)
- [Theme development kit](https://developers.finqu.com/apis-and-tools/theme-dev/overview.md.txt)
