# Configuration

The CLI stores settings in a `.env` file as `FINQU_`-prefixed variables. `finqu theme configure` and `finqu sign-in` write this file. The default path is `./.env`. Override it with `--config` or `FINQU_CONFIG`.

```env
FINQU_RESOURCE_URL=https://example.api.myfinqu.com
FINQU_STORE_DOMAIN=example.finqustore.com
FINQU_STORE_THEME_ID=870
FINQU_ACCESS_TOKEN=
FINQU_REFRESH_TOKEN=
```

| Variable | Role |
|----------|------|
| `FINQU_RESOURCE_URL` | Admin API base URL, set by `finqu sign-in` |
| `FINQU_STORE_DOMAIN` | Connected store domain |
| `FINQU_STORE_THEME_ID` | Connected theme id |
| `FINQU_API_VERSION` | Optional. Default in the CLI docs is `1.2` |
| `FINQU_THEME_DIR` | Local theme directory |

Do not commit `.env`.

## Legacy JSON

Older projects used `finqu.config.json`. Convert it:

```bash
finqu migrate
```

## Full reference

- [CLI configuration](https://developers.finqu.com/apis-and-tools/cli/configuration.md.txt)
