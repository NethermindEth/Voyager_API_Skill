# Rate Limits

## Overview

The Voyager API enforces rate limits per API key to ensure fair usage and platform stability. All developers share the same infrastructure, so rate limits protect the quality of service for everyone.

---

## How Rate Limits Work

- Rate limits are applied **per API key**, not per IP address
- Each API key has a maximum number of requests allowed within a time window
- When the limit is exceeded, subsequent requests are rejected until the window resets
- Different API key tiers may have different rate limit thresholds

---

## Rate Limit Response

When you exceed the rate limit, the API returns:

```
HTTP 429 Too Many Requests
```

The response body will indicate that the rate limit has been exceeded. No data is returned for rate-limited requests.

---

## Handling Rate Limits

Follow this sequence when you receive a `429` response:

1. **Detect** — Check the HTTP status code for `429`
2. **Wait** — Do not retry immediately
3. **Backoff** — Calculate a wait time using exponential backoff
4. **Retry** — Attempt the request again after waiting
5. **Give up** — After a maximum number of retries, surface the error

---

## Exponential Backoff Strategy

Use exponential backoff with jitter to avoid synchronized retry storms:

| Retry Attempt | Base Wait | With Jitter (example) |
|---|---|---|
| 1st | 1 second | 0.8–1.2 seconds |
| 2nd | 2 seconds | 1.6–2.4 seconds |
| 3rd | 4 seconds | 3.2–4.8 seconds |
| 4th | 8 seconds | 6.4–9.6 seconds |
| 5th | 16 seconds | 12.8–19.2 seconds |

### Python Example

```python
import time
import random
import requests

def fetch_with_backoff(url, headers, max_retries=5):
    for attempt in range(max_retries):
        response = requests.get(url, headers=headers)
        if response.status_code != 429:
            return response

        wait = (2 ** attempt) + random.uniform(-0.2, 0.2)
        time.sleep(wait)

    raise Exception("Rate limit exceeded after max retries")
```

### JavaScript Example

```javascript
async function fetchWithBackoff(url, headers, maxRetries = 5) {
  for (let attempt = 0; attempt < maxRetries; attempt++) {
    const response = await fetch(url, { headers });
    if (response.status !== 429) return response;

    const wait = Math.pow(2, attempt) + (Math.random() * 0.4 - 0.2);
    await new Promise(resolve => setTimeout(resolve, wait * 1000));
  }
  throw new Error("Rate limit exceeded after max retries");
}
```

---

## Prevention Tips

- **Spread requests over time** — Avoid sending bursts of requests in quick succession
- **Cache responses locally** — Store data that doesn't change frequently to avoid repeated requests
- **Use appropriate page sizes** — Fewer, well-sized pages reduce total request count
- **Avoid tight polling loops** — Add delays between repeated calls to the same endpoint
- **Batch processing** — Collect work and process in intervals rather than per-item requests
- **Monitor your usage** — Track your request count to stay within limits proactively

---

## Notes

- Rate limits exist to protect platform stability; they are not punitive
- If you consistently hit rate limits, consider optimizing your request patterns first
- Contact Voyager support if you need higher limits for production workloads or enterprise use cases
- Rate-limited requests (429) should not be counted as errors in your application metrics — they are expected under load

---

## Related Files

- `common-errors.md` — Full HTTP error code reference
- `best-practices.md` — Request efficiency and caching strategies
- `pagination.md` — Balancing page sizes with request count
