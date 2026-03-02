# Common Errors

## Overview

The Voyager API uses standard HTTP status codes to indicate the success or failure of requests. This file covers the most common error responses, their causes, and how to resolve them.

---

## Error Reference

### 400 Bad Request

**Cause:** The request contains invalid or malformed query parameters.

**Common triggers:**
- Invalid value for `p` or `ps` (e.g., negative numbers, non-integers)
- Malformed block hash, transaction hash, or contract address
- Unsupported query parameter values

**Fix:**
- Validate all parameters before sending the request
- Ensure hashes and addresses are valid hex strings prefixed with `0x`
- Check parameter types match the expected format (integer, string, etc.)

---

### 401 Unauthorized

**Cause:** The request is missing the `x-api-key` header, or the provided API key is invalid or revoked.

**Common triggers:**
- Forgot to include the `x-api-key` header
- API key is misspelled or truncated
- API key has been revoked or expired
- Key passed as a query parameter instead of a header

**Fix:**
- Verify the `x-api-key` header is included in every request
- Confirm the key matches what's shown in your Voyager dashboard
- Generate a new key if the current one may be compromised

---

### 404 Not Found

**Cause:** The requested resource does not exist.

**Common triggers:**
- Transaction hash, block number, or contract address doesn't exist on Starknet
- Typo in the URL path or resource identifier
- Requesting data that hasn't been indexed yet

**Fix:**
- Double-check the resource identifier for typos
- Verify the resource exists using the Voyager block explorer (https://voyager.online)
- For very recent data, wait briefly and retry (indexing delay)

---

### 429 Too Many Requests

**Cause:** The rate limit for your API key has been exceeded.

**Common triggers:**
- Sending too many requests in a short time window
- Tight polling loops without delays
- Burst traffic from parallelized scripts

**Fix:**
- Implement exponential backoff with jitter (see `rate-limits.md`)
- Spread requests over time
- Cache responses for data that doesn't change frequently
- Reduce page sizes or request frequency

---

### 500 Internal Server Error

**Cause:** An unexpected error occurred on the Voyager server.

**Common triggers:**
- Temporary server issue
- Edge case in request processing

**Fix:**
- Retry the request after a brief delay
- If the error persists, check Voyager's status page or contact support
- Do not retry immediately in a tight loop

---

## Error Handling Best Practices

1. **Always check HTTP status codes** before parsing response bodies
2. **Handle each error type explicitly** — don't treat all errors the same
3. **Log the status code and response body** for failed requests to aid debugging
4. **Validate inputs** (hashes, addresses, parameter types) before making requests
5. **Implement retry logic** with backoff for transient errors (429, 500)
6. **Set timeouts** on HTTP requests to avoid hanging on slow responses
7. **Surface meaningful errors** to your users instead of raw API responses

---

## Quick Reference Table

| Status Code | Meaning | Retryable | Action |
|---|---|---|---|
| `200` | Success | — | Process response |
| `400` | Bad Request | No | Fix parameters |
| `401` | Unauthorized | No | Fix API key |
| `404` | Not Found | No | Verify resource exists |
| `429` | Too Many Requests | Yes (with backoff) | Wait and retry |
| `500` | Internal Server Error | Yes (with delay) | Retry, then contact support |

---

## Related Files

- `authentication.md` — API key setup and troubleshooting
- `rate-limits.md` — Rate limit details and backoff strategies
- `best-practices.md` — Error handling and production guidance
- `api-overview.md` — Request and response format
