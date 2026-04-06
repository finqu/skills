# App Context

How to validate and use app context when your app runs embedded in the Finqu admin.

## Overview

When a merchant opens your app inside the admin, Finqu provides a **context token** (JWT) that identifies:

- Which merchant is using your app
- The current session
- App permissions

## Validating the Context Token

On every request from your embedded app:

1. Extract the context JWT from the request
2. Validate the JWT signature using your **client secret**
3. Check the JWT hasn't expired
4. Read the merchant ID from the `sub` claim

```typescript
import jwt from 'jsonwebtoken';

function validateContext(token, clientSecret) {
    try {
        const decoded = jwt.verify(token, clientSecret);
        return {
            merchantId: decoded.sub,
            // ... other claims
        };
    } catch (error) {
        throw new Error('Invalid context token');
    }
}
```

## Security

- **Always validate** the context token on your backend — never trust the frontend alone
- Use your **client secret** for validation — keep it secure
- Check token expiration
- Scope all API calls to the merchant ID from the token

## Full Reference

- [App context](https://developers.finqu.com/build-with-finqu/apps-integrations/app-context.md.txt)
