# Pagination

## Overview

All list endpoints in the Voyager API return paginated results. Pagination is required to efficiently retrieve large datasets without overloading the API or your application.

---

## Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `p` | integer | Yes | Page number (starts at **1**, not 0) |
| `ps` | integer | Yes | Page size (number of items per page) |

Always include **both** `p` and `ps` in every list request for predictable behavior. Omitting them may return default-sized results.

---

## Examples

First page of 10 items:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/blocks?p=1&ps=10"
```

Second page of 10 items:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/blocks?p=2&ps=10"
```

---

## Detecting the Last Page

There is no explicit `totalPages` or `hasMore` field in the response. Instead, determine the last page by inspecting the number of returned items:

- If the response contains **exactly `ps` items** → more pages may exist
- If the response contains **fewer than `ps` items** → this is the last page
- If the response contains **zero items** → no data exists for this page

---

## Iterating Through All Data

To retrieve all records for a given query:

```
1. Set p = 1
2. Fetch the page
3. Process the returned items
4. If the number of returned items equals ps:
     → increment p by 1 and go to step 2
5. If the number of returned items is less than ps:
     → stop (you have reached the end)
```

### Python Example

```python
import requests

headers = {"x-api-key": "YOUR_API_KEY"}
base_url = "https://api.voyager.online/beta"

page = 1
page_size = 25
all_blocks = []

while True:
    response = requests.get(
        f"{base_url}/blocks",
        params={"p": page, "ps": page_size},
        headers=headers
    )
    items = response.json().get("items", [])
    all_blocks.extend(items)

    if len(items) < page_size:
        break
    page += 1
```

---

## Recommended Page Sizes

| Use Case | Recommended `ps` |
|---|---|
| Quick lookups / previews | 5–10 |
| Standard listing | 10–25 |
| Bulk data retrieval | 25–50 |
| Maximum throughput | 50 (avoid larger sizes) |

- **Avoid very large page sizes.** They increase response time, memory usage, and risk of timeouts.
- **Smaller pages** improve perceived latency and reduce the impact of a failed request.
- **Balance page size with rate limits.** Fewer, larger pages mean fewer requests but heavier payloads.

---

## Common Mistakes

| Mistake | Problem | Fix |
|---|---|---|
| Starting at `p=0` | Returns unexpected results or empty data | Start at `p=1` |
| Omitting `ps` | Unpredictable page sizes | Always include `ps` |
| Using `ps=1000` | Slow responses, potential timeouts | Use `ps=50` or less |
| Not checking item count | Infinite loop when no stop condition | Stop when items < `ps` |

---

## Notes

- Pagination applies to all list endpoints: `/blocks`, `/txns`, `/events`, `/tokens/transfers`
- Single-item lookups (e.g., `/txns/{hash}`) do not use pagination
- Results are ordered from most recent to oldest by default
- Page numbering starts at **1**, not **0**

---

## Related Files

- `api-overview.md` — Base URL and general request format
- `rate-limits.md` — Balancing pagination with rate limits
- `best-practices.md` — Efficient request patterns
