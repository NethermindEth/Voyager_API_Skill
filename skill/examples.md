# Example Queries

## Overview

This file contains example user questions and recommended responses. Use these as a reference for how to answer common Voyager API integration questions.

---

## Fetching Blocks

**User:** "How do I get the latest 5 blocks?"

**Answer:**

Use the blocks endpoint with page size 5:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/blocks?p=1&ps=5"
```

Blocks are returned in reverse chronological order, so page 1 contains the most recent blocks. Always include the `x-api-key` header.

---

## Fetching a Specific Transaction

**User:** "How do I look up a transaction by its hash?"

**Answer:**

Use the transaction detail endpoint with the full transaction hash:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/txns/0x04a3c..."
```

The response includes the transaction type, status, sender, fee, and block number. The `fee` is a raw integer in wei — divide by 10^18 for ETH.

---

## Filtering Transactions by Type

**User:** "How do I get only INVOKE transactions?"

**Answer:**

Use the `type` filter on the transactions list endpoint:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/txns?p=1&ps=10&type=INVOKE"
```

Supported types: `INVOKE`, `DEPLOY`, `DEPLOY_ACCOUNT`, `DECLARE`, `L1_HANDLER`.

---

## Querying Token Transfers

**User:** "How do I get ERC20 transfers for a specific token?"

**Answer:**

Use the token transfers endpoint with the `contract` filter:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/tokens/transfers?p=1&ps=10&contract=0x049d36..."
```

Token values are returned as raw integers. Apply the token's `decimals` to get human-readable amounts.

---

## Looking Up a Contract

**User:** "How do I get details about a deployed contract?"

**Answer:**

Use the contracts endpoint with the contract address:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts/0x049d36..."
```

The response includes the class hash, contract type, deployment block, and deploy transaction hash.

---

## Querying Events

**User:** "How do I find all Transfer events from a specific contract?"

**Answer:**

Use the events endpoint with both `contract` and `name` filters:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/events?p=1&ps=10&contract=0x049d36...&name=Transfer"
```

Event keys and data are returned as raw felt values. Decode them according to the contract's ABI.

---

## Handling Rate Limits

**User:** "Why am I getting 429 errors?"

**Answer:**

HTTP `429` means you've exceeded the rate limit for your API key. To fix this:

1. **Do not retry immediately** — wait before sending the next request
2. **Implement exponential backoff** — wait 1s, then 2s, then 4s between retries
3. **Add jitter** — randomize wait times slightly to avoid synchronized retries
4. **Reduce request frequency** — cache responses and spread requests over time

See `rate-limits.md` for a full backoff strategy with code examples.

---

## Paginating Through All Results

**User:** "How do I get all transactions, not just the first page?"

**Answer:**

Iterate through pages until you receive fewer items than your page size:

```python
import requests

headers = {"x-api-key": "YOUR_API_KEY"}
page = 1
page_size = 25
all_txns = []

while True:
    response = requests.get(
        "https://api.voyager.online/beta/txns",
        params={"p": page, "ps": page_size},
        headers=headers
    )
    items = response.json().get("items", [])
    all_txns.extend(items)

    if len(items) < page_size:
        break
    page += 1
```

Start at `p=1` (not 0). Stop when the response has fewer items than `ps`. See `pagination.md` for more details.

---

## Authentication Issues

**User:** "I'm getting 401 Unauthorized. What's wrong?"

**Answer:**

HTTP `401` means your API key is missing or invalid. Check the following:

1. **Include the header** — every request needs `x-api-key: YOUR_API_KEY`
2. **Verify the key** — confirm it matches your Voyager dashboard exactly
3. **Check placement** — the key must be in a header, not a query parameter
4. **Check for revocation** — the key may have been revoked; generate a new one if needed

See `authentication.md` for setup details and code examples.

---

## Checking Contract Class Type

**User:** "How do I tell if a contract uses Cairo 0 or Cairo 1?"

**Answer:**

Look up the contract's class hash, then check the class type:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes/0x03e327..."
```

The `type` field will be:
- `SIERRA` — Cairo 1+ (current standard)
- `LEGACY` — Cairo 0 (deprecated)

First get the class hash from the contract details endpoint, then use it to query the class.

---

## Related Files

- `api-overview.md` — Endpoints, base URL, and request format
- `authentication.md` — API key setup and security
- `blocks.md` — Block endpoint reference
- `transactions.md` — Transaction endpoint reference
- `contracts.md` — Contract and class endpoint reference
- `tokens.md` — Token transfer endpoint reference
- `events.md` — Event endpoint reference
- `pagination.md` — Pagination patterns and code examples
- `rate-limits.md` — Rate limit handling and backoff strategy
- `common-errors.md` — Error codes and troubleshooting
