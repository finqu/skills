# Your First App

Step-by-step guide to building a Finqu partner app from scratch.

## Step 1: Create the App in Partner

1. Open Finqu Partner dashboard and create a new app
2. Note the **app ID** (needed for App Link)
3. Configure:
    - **Install URL** — Public HTTPS URL where merchants land to start installation
    - **Redirect URI(s)** — OAuth callback URLs
    - **Scopes** — API permissions your app needs
4. App starts in **draft** state (won't appear in listings)

## Step 2: Copy Credentials

From the Partner app settings:

- **Client ID** (`client_id`) — Used in OAuth authorization URL and token exchange
- **Client secret** — Used for token exchange and validating app context JWT

**Store in environment variables — never commit to version control.**

## Step 3: Set Up Your Project

```bash
npm install @finqu/app-link @finqu/cool
```

- `@finqu/app-link` — PostMessage bridge to Finqu admin
- `@finqu/cool` — Finqu's UI component library

Use any frontend stack that can load in an iframe (React, Vue, plain JS). Your app is served from your own domain and embedded in the admin.

## Step 4: Set Up App Link

```typescript
import { App } from '@finqu/app-link';

const app = App.create('your-app-id', 'https://admin.example.myfinqu.com');

app.ready(() => {
    console.log('App instance ID:', app.getId());
    console.log('Session ID:', app.getSessionId());
});
```

## Step 5: Start Coding

**Backend:**

1. Implement the install endpoint
2. Redirect to the OAuth authorization URL
3. Handle the callback — exchange code for token
4. Store credentials securely
5. Show success page

**Embedded UI:**

1. Validate context token (JWT) on every request using client secret
2. Read merchant ID from JWT `sub` claim

**Frontend:**

1. Use App Link's `subscribe` and `dispatch` for host communication
2. Open dialogs, load resources, trigger navigation

Submit for review when ready to move from draft to private or public.

## Full Reference

- [Your first app](https://developers.finqu.com/build-with-finqu/apps-integrations/your-first-app.md.txt)
- [App installation & authentication](https://developers.finqu.com/build-with-finqu/apps-integrations/app-installation-authentication.md.txt)
