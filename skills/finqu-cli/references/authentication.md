# Authentication

How to authenticate with the Finqu platform using the CLI.

## Sign in

```bash
finqu sign-in
```

Without `--key` and `--secret`, the CLI opens a browser for OAuth 2.0 and writes credentials to `.env`.

With API credentials (Apps page for a merchant user, Partner management for a partner):

```bash
finqu sign-in --key YOUR_API_KEY --secret YOUR_API_SECRET
```

Environment alternatives: `FINQU_API_CLIENT_ID` and `FINQU_API_CLIENT_SECRET`.

## Refresh

There is no `logout` command. Run `finqu sign-in` again when the session fails. The CLI also refreshes tokens when they expire.

## Full reference

- [CLI authentication](https://developers.finqu.com/apis-and-tools/cli/authentication.md.txt)
