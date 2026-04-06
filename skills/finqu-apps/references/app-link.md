# App Link

App Link enables communication between your Finqu app (running in an iframe) and the Finqu admin host.

## Why App Link?

When your app runs inside an iframe in the admin, it cannot directly access the parent window (browser security). App Link provides a safe PostMessage bridge to:

- Receive context from the host (app instance ID, session ID)
- React to host actions (open dialog, navigate, load resource)
- Send messages back to the host (confirmed, saved, resize)

Only the intended admin origin can communicate with your app — origin validation is built in.

## Installation

```bash
npm install @finqu/app-link
```

```typescript
import { App, ComponentGroup, ResourceType } from '@finqu/app-link';
```

## Basic Usage

```typescript
const app = App.create('your-app-id', 'https://admin.example.myfinqu.com');

app.ready(() => {
    console.log('App instance ID:', app.getId());
    console.log('Session ID:', app.getSessionId());
});
```

## API

- `App.create(appId, adminOrigin)` — Create app instance
- `app.ready(callback)` — Fires when admin acknowledges the app
- `app.getId()` — App instance ID
- `app.getSessionId()` — Current session ID
- `app.subscribe(event, handler)` — Listen for host events
- `app.dispatch(action)` — Send actions to the host

## Component Groups and Resources

App Link supports interaction with:

- **Dialog** — Open modals, respond to confirm/close
- **Navigation** — Trigger navigation in the admin
- **Resource** — Load products, orders, customers
- **Checkout** — Interact with checkout flows

## When to Use

Use App Link for apps that:

- Render inside the Finqu admin (embedded in iframe)
- Need to show dialogs, load resources, or trigger navigation
- Should respond to host events

**Not needed** if your app runs standalone outside the admin.

## Full Reference

- [App Link introduction](https://developers.finqu.com/build-with-finqu/apps-integrations/app-link/introduction.md.txt)
- [App Link getting started](https://developers.finqu.com/build-with-finqu/apps-integrations/app-link/getting-started.md.txt)
- [App Link API](https://developers.finqu.com/build-with-finqu/apps-integrations/app-link/app-api.md.txt)
- [App Link messaging](https://developers.finqu.com/build-with-finqu/apps-integrations/app-link/messaging.md.txt)
- [Component groups & resources](https://developers.finqu.com/build-with-finqu/apps-integrations/app-link/component-groups-and-resources.md.txt)
