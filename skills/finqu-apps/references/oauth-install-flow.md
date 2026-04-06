# OAuth Install Flow

How the OAuth 2.0 install flow works for Finqu partner apps.

## Flow Summary

1. Merchant clicks "Install" → your **Install URL**
2. Your app redirects to Finqu's authorization endpoint:
    ```
    https://admin.myfinqu.com/oauth/authorize?
      client_id=YOUR_CLIENT_ID
      &redirect_uri=YOUR_REDIRECT_URI
      &scope=read_products,write_orders
      &response_type=code
    ```
3. Merchant reviews and approves requested scopes
4. Finqu redirects to your **Redirect URI** with an authorization `code`
5. Your backend exchanges the code for an access token:

    ```http
    POST /oauth/token
    Content-Type: application/json

    {
      "client_id": "YOUR_CLIENT_ID",
      "client_secret": "YOUR_CLIENT_SECRET",
      "code": "AUTHORIZATION_CODE",
      "grant_type": "authorization_code",
      "redirect_uri": "YOUR_REDIRECT_URI"
    }
    ```

6. Store the access token securely and associate with the merchant

## Security

- Validate the `state` parameter to prevent CSRF
- Exchange codes promptly — they expire quickly
- Store tokens encrypted at rest
- Never expose client secret in frontend code
- Use HTTPS for all redirect URIs

## Full Reference

- [App installation & authentication](https://developers.finqu.com/build-with-finqu/apps-integrations/app-installation-authentication.md.txt)
