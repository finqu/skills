---
name: finqu-cli
description: 'Finqu CLI — theme development, storefront scaffolding, authentication, and local development workflow'
---

# Finqu CLI

## When to use

Use this skill when:

- Installing or configuring the Finqu CLI (`@finqu/cli`)
- Authenticating with the Finqu platform via CLI
- Developing Liquid themes locally (configure, dev, deploy)
- Creating or managing headless storefronts via CLI
- Troubleshooting CLI issues

## Inputs required

- **Node.js**: v18+ installed
- **Finqu account**: Valid merchant or partner credentials
- **Store access**: Store URL and credentials for the target store

## Procedure

### 0) Install the CLI

```bash
npm install -g @finqu/cli
```

Verify installation:

```bash
finqu --version
```

### 1) Authenticate

```bash
finqu sign-in
```

Opens a browser for OAuth sign-in when you omit credentials. With an API key and secret:

```bash
finqu sign-in --key YOUR_API_KEY --secret YOUR_API_SECRET
```

The CLI stores the session in the project `.env`.

Read: `references/authentication.md`

### 2) Choose your workflow

**Liquid theme development:**

1. Configure the theme connection
2. Start local development server
3. Edit and preview changes
4. Deploy when ready

→ Steps 3–5 below

**Headless storefront:**

1. Create a new storefront project
2. Start development server
3. Deploy

→ Step 6 below

### 3) Configure a theme

```bash
finqu theme configure
```

This connects your local directory to a specific theme on a specific store. It writes the store and theme into `.env` (`FINQU_STORE_DOMAIN`, `FINQU_STORE_THEME_ID`, and related `FINQU_` keys).

A legacy `finqu.config.json` converts with `finqu migrate`.

Read: `references/theme-commands.md`

### 4) Develop the theme locally

```bash
finqu theme dev
```

Starts a local development server that:

- Watches for file changes
- Syncs changes to the connected store
- Provides a preview URL

### 5) Deploy the theme

```bash
finqu theme deploy
```

Uploads the current theme files to the connected store. The theme becomes live (or updates the draft, depending on settings).

### 6) Create a storefront (headless)

```bash
finqu storefront create
```

Scaffolds a new headless storefront project. Follow the interactive prompts to configure the project.

Read: `references/storefront-commands.md`

### 7) Configure your project

After sign-in and `finqu theme configure`, the project `.env` holds the connection. Override the file with `--config` or `FINQU_CONFIG`.

```env
FINQU_STORE_DOMAIN=example.finqustore.com
FINQU_STORE_THEME_ID=870
FINQU_RESOURCE_URL=https://example.api.myfinqu.com
```

Read: `references/configuration.md`

## Verification

- `finqu --version` outputs the installed version
- `finqu sign-in` completes and writes credentials to `.env`
- `finqu theme configure` stores the store domain and theme id in `.env`
- `finqu theme dev` starts the dev server and opens a preview
- `finqu theme deploy` uploads files without errors
- `finqu storefront create` scaffolds a new project

## Failure modes / debugging

- **"command not found: finqu"**: Not installed globally — run `npm install -g @finqu/cli`
- **Authentication fails**: Run `finqu sign-in` again. There is no `logout` command
- **Theme configure fails**: Verify you are signed in and that the account can access the store
- **Dev server errors**: Check Node.js version (v18+ required), check port availability
- **Deploy fails**: Verify config, check network, ensure theme exists on the store

Read: `references/troubleshooting.md`

## Escalation

- See [CLI overview](https://developers.finqu.com/apis-and-tools/cli/overview.md.txt)
- See [CLI authentication](https://developers.finqu.com/apis-and-tools/cli/authentication.md.txt)
- See [Theme commands](https://developers.finqu.com/apis-and-tools/cli/theme.md.txt)
- See [Theme development kit](https://developers.finqu.com/apis-and-tools/theme-dev/overview.md.txt)
