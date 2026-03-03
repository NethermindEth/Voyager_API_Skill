# Blocks Endpoint

## Overview

The blocks endpoint provides access to Starknet block data. You can list blocks with pagination or retrieve a specific block by its number or hash.

---

## List Blocks

```
GET /blocks
```

Returns a paginated list of Starknet blocks, ordered from most recent to oldest.

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `p` | integer | Yes | Page number (starts at 1) |
| `ps` | integer | Yes | Page size (items per page) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/blocks?p=1&ps=10"
```

---

## Get Block by Hash

```
GET /blocks/{blockHash}
```

Returns details for a specific block identified by its block hash.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/blocks/0x19402ac841080ad4849ac0209265cb6e82856036ffce993e7f583b322c61092"
```

---

## Response Fields

| Field | Type | Description |
|---|---|---|
| `hash` | string | Unique block hash |
| `blockNumber` | integer | Sequential block number |
| `timestamp` | integer | Unix timestamp of when the block was produced |
| `txnCount` | integer | Number of transactions included in the block |
| `eventCount` | integer | Number of events included in the block |
| `messageCount` | integer | Number of messages included in the block |
| `status` | string | Block status (e.g., ACCEPTED_ON_L2, ACCEPTED_ON_L1) |
| `l1VerificationTxHash` | string/null | l1 verification hash |
| `stateRoot` | string | State root after this block |
| `sequencerAddress` | string | Address of the sequencer that produced this block |

---

## Notes

- Blocks are returned in reverse chronological order (newest first) when listing
- The most recent block may have a slight delay due to indexing (eventual consistency)
- Block numbers are sequential and unique; block hashes are unique hex strings
- Use `p=1&ps=1` to fetch only the latest indexed block

---

## Related Files

- `transactions.md` — Query transactions within a block
- `events.md` — Query events emitted in a block's transactions
- `pagination.md` — Pagination parameters and iteration strategy
- `api-overview.md` — Base URL and request format
