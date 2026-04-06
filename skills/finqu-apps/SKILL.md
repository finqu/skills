---
name: finqu-apps
description: 'Building Finqu partner apps and integrations — OAuth, App Link, embedding in admin, publishment'
---

# Finqu Apps & Integrations

## When to use

Use this skill when:

- Building a partner app that other merchants can install
- Building a custom integration for a specific merchant
- Implementing the OAuth install flow
- Embedding an app inside the Finqu admin via iframe
- Using App Link for communication between app and admin
- Publishing an app (draft → private → public)

## Inputs required

- **App type**: Partner app (installable) or custom integration (merchant-specific)
- **Partner dashboard access**: For partner apps
- **Client ID and secret**: From Partner app settings
- **App ID**: For App Link initialization

## Procedure

### 0) Choose your path

**Custom integration** (merchant-specific):

- Uses REST API directly
- Auth tied to specific merchant
- Not listed in app store
- → See **finqu-rest-api** skill for API details

**Partner app** (installable by others):

- Registered in Partner dashboard
- Uses OAuth install flow
- Can be published (private link or public listing)
- → Continue with this skill

### 1) Create the app in Partner

1. Open Finqu Partner dashboard
2. Create a new app
3. Note the **app ID** (needed for App Link)
4. Configure app settings:
    - **Install URL**: public HTTPS URL where merchants land
    - **Redirect URI(s)**: OAuth redirect URLs
    - **Scopes**: API permissions your app needs
5. Copy **Client ID** and **Client secret** — store in environment variables

Read: `references/your-first-app.md`

### 2) Set up your project

```bash
npm install @finqu/app-link @finqu/cool
```

- `@finqu/app-link` — PostMessage communication with Finqu admin
- `@finqu/cool` — Finqu's UI library for admin interfaces

Use any frontend framework (React, Vue, plain JS) loadable in an iframe. Your app is served from your own domain, embedded in the admin.

### 3) Implement the OAuth install flow

1. Merchant visits your Install URL
2. Redirect to Finqu authorization endpoint with `client_id` and scopes
3. Merchant approves access
4. Finqu redirects back to your Redirect URI with an authorization code
5. Exchange the code for an access token using your client secret
6. Store credentials securely and show success

Read: `references/oauth-install-flow.md`

### 4) Set up App Link

```typescript
import { App } from '@finqu/app-link';

const app = App.create('your-app-id', 'https://admin.example.myfinqu.com');

app.ready(() => {
    console.log('App instance ID:', app.getId());
    console.log('Session ID:', app.getSessionId());
});
```

- `ready()` fires when the admin host acknowledges your app
- Use `subscribe` to listen for host events (dialogs, navigation, resources)
- Use `dispatch` to send messages back (resize, navigate, etc.)

Read: `references/app-link.md`

### 5) Handle app context

When your app runs embedded in the admin, validate the context token:

1. Every request includes a context JWT
2. Validate the JWT using your **client secret**
3. Read the merchant ID from the JWT `sub` claim
4. Use this to scope API calls to the correct merchant

Read: `references/app-context.md`

### 6) Publish your app

1. **Draft** — Default state, not visible to others
2. **Private** — Shareable via link (requires review)
3. **Public** — Listed in app store (requires review)

Read: `references/app-publishment.md`

## Verification

- OAuth flow completes: install URL → authorization → token exchange → success
- App Link initializes: `ready()` callback fires
- Context token validates correctly with client secret
- API calls work with the obtained access token
- App renders correctly inside the Finqu admin iframe
- App works across different merchants (if partner app)

## Failure modes / debugging

- **OAuth redirect fails**: Check Redirect URI matches exactly in Partner settings
- **Context token invalid**: Verify client secret is correct, check JWT expiry
- **App Link not connecting**: Ensure admin origin matches `App.create()` second parameter
- **API calls unauthorized**: Token may have expired — implement token refresh
- **iframe blocked**: Ensure your server sends correct `X-Frame-Options` / CSP headers allowing Finqu admin origin

## Escalation

- See [Apps & integrations overview](https://developers.finqu.com/build-with-finqu/apps-integrations/overview.md.txt)
- See [Your first app guide](https://developers.finqu.com/build-with-finqu/apps-integrations/your-first-app.md.txt)
- See [App Link introduction](https://developers.finqu.com/build-with-finqu/apps-integrations/app-link/introduction.md.txt)
- See [App context](https://developers.finqu.com/build-with-finqu/apps-integrations/app-context.md.txt)
