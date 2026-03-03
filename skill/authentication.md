# Authentication

## Overview

All Voyager API requests require authentication via an API key. Unauthenticated requests will receive an HTTP `401 Unauthorized` response.

---

## Required Header

Include the following header with every request:

```
x-api-key: YOUR_API_KEY
```

Replace `YOUR_API_KEY` with your actual Voyager API key.

---

## Obtaining an API Key

API keys can be obtained through the Voyager developer portal at https://voyager.online. Register for an account and generate a key from your dashboard.

---

## Examples

### curl

```
curl -H "x-api-key: YOUR_API_KEY" \
     https://api.voyager.online/beta/blocks?p=1&ps=10
```

### HTTP

```
GET /blocks?p=1&ps=10 HTTP/1.1
Host: api.voyager.online
x-api-key: YOUR_API_KEY
```

### Python (requests)

```python
import requests

headers = {"x-api-key": "YOUR_API_KEY"}
response = requests.get(
    "https://api.voyager.online/beta/blocks",
    params={"p": 1, "ps": 10},
    headers=headers
)
data = response.json()
```

### JavaScript (fetch)

```javascript
const response = await fetch(
  "https://api.voyager.online/beta/blocks?p=1&ps=10",
  { headers: { "x-api-key": "YOUR_API_KEY" } }
);
const data = await response.json();
```

---

## Security Guidelines

- **Never expose API keys in frontend or client-side code.** Keys embedded in browser JavaScript are visible to anyone inspecting the page.
- **Store keys in environment variables.** Use `.env` files or your platform's secrets manager (e.g., AWS Secrets Manager, Vercel Environment Variables).
- **Do not commit keys to version control.** Add `.env` to your `.gitignore` file.
- **Rotate keys periodically.** Generate new keys and revoke old ones on a regular cadence.
- **Use separate keys for development and production.** This limits blast radius if a key is compromised.
- **Never log full API keys.** If logging is necessary, mask all but the last 4 characters.

---

## Troubleshooting

| Symptom | Cause | Fix |
|---|---|---|
| HTTP `401 Unauthorized` | Missing `x-api-key` header | Add the header to your request |
| HTTP `401 Unauthorized` | Invalid or revoked API key | Verify the key in your Voyager dashboard |
| HTTP `401 Unauthorized` | Key passed as query parameter instead of header | Move the key to the `x-api-key` header |

---

## Related Files

- `api-overview.md` — Request format and base URL
- `common-errors.md` — Full error code reference
- `best-practices.md` — Security and integration guidance
