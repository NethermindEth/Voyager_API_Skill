# Best Practices

## Overview

This file provides production-grade guidance for integrating with the Voyager API. Following these practices will help you build reliable, efficient, and secure applications.

---

## Authentication

- **Store API keys in environment variables**, never hardcoded in source code
- **Never expose API keys in frontend or client-side code** — keys in browser JavaScript are visible to anyone
- **Do not commit keys to version control** — add `.env` to your `.gitignore`
- **Rotate keys periodically** and revoke unused or compromised keys
- **Use separate keys** for development, staging, and production environments
- **Never log full API keys** — mask all but the last 4 characters if logging is necessary

---

## Request Efficiency

- **Request only the data you need** — use specific endpoints and filters instead of fetching everything
- **Use appropriate page sizes** (`ps=10` to `ps=50`) to balance payload size and request count
- **Cache responses locally** for data that doesn't change frequently (e.g., historical blocks, old transactions)
- **Avoid polling the same endpoint in tight loops** — add reasonable intervals between repeated calls
- **Batch your data processing** — collect work and process in intervals rather than one request per item
- **Use filters** (`block`, `type`, `contract`, `name`) to narrow results server-side rather than filtering client-side

---

## Pagination

- **Always include both `p` and `ps`** parameters for predictable behavior
- **Start at page 1**, not page 0
- **Stop paginating** when the response returns fewer items than `ps`
- **Use small page sizes** for interactive UIs; larger sizes for background data ingestion
- See `pagination.md` for a full iteration algorithm and code examples

---

## Rate Limiting

- **Respect rate limits** — do not retry immediately on HTTP `429` responses
- **Implement exponential backoff with jitter** to avoid synchronized retry storms
- **Spread requests over time** instead of sending bursts
- **Monitor your request count** to stay within limits proactively
- **Set a maximum retry count** (e.g., 5 attempts) to prevent infinite retry loops
- See `rate-limits.md` for a detailed backoff strategy and code examples

---

## Error Handling

- **Check HTTP status codes before parsing** response bodies
- **Handle each error type explicitly**: `401` (auth), `429` (rate limit), `400` (bad params), `404` (not found), `500` (server error)
- **Log failed requests** with the status code, response body, and request URL for debugging
- **Validate input parameters** (block hashes, addresses, page numbers) before sending requests
- **Set HTTP timeouts** to avoid hanging on slow or unresponsive requests
- **Surface meaningful errors** to end users instead of raw API responses
- See `common-errors.md` for a full error reference

---

## Data Consistency

- **Voyager serves indexed data**, which is **eventually consistent** with the Starknet chain
- **Do not assume real-time accuracy** — the most recently produced blocks may have a slight indexing delay
- **Confirm critical data against an RPC node** if your use case requires exact finality guarantees
- **Historical data is stable** — blocks, transactions, and events from older blocks will not change
- **Token values are raw integers** — always apply the token's `decimals` before displaying amounts to users

---

## Security

- **Use HTTPS exclusively** — the base URL is `https://api.voyager.online/beta`
- **Never log full API keys** in application logs or error tracking systems
- **Restrict API key scope** to the minimum permissions needed
- **Validate and sanitize** all user-supplied inputs (addresses, hashes) before passing them to the API
- **Do not trust API responses blindly** in security-critical paths — validate data integrity where needed

---

## Application Architecture

- **Decouple API calls from your UI** — use a backend proxy or API layer to avoid exposing keys in the browser
- **Implement a caching layer** (e.g., Redis, in-memory cache) between your application and the Voyager API
- **Use connection pooling** for HTTP clients to reduce overhead on repeated requests
- **Design for failure** — assume any request can fail and build retry/fallback logic accordingly
- **Log and monitor** API response times and error rates to detect issues early

---

## Related Files

- `authentication.md` — API key setup and security
- `pagination.md` — Pagination parameters and iteration patterns
- `rate-limits.md` — Rate limit policies and backoff strategy
- `common-errors.md` — Error codes and troubleshooting
- `api-overview.md` — Base URL, endpoints, and request format
