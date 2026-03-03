# Events Endpoint

## Overview

The events endpoint provides access to events emitted by Starknet contracts. Events are the primary mechanism contracts use to communicate state changes and are essential for indexing, monitoring, and building reactive applications.

---

## List Events

```
GET /events
```

Returns a paginated list of events emitted across all contracts, ordered from most recent to oldest.

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `p` | integer | No | Page number (starts at 1) |
| `ps` | integer | No | Page size (items per page) |
| `contract` | string | No | Filter by emitting contract address |
| `txnHash` | string | No | Filter events by transaction hash. Cannot be used with contract |
| `blockHash` | string | No | Filter events by transaction hash. Cannot be used with contract |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/events?p=1&ps=10"
```

### Filtered Examples

Events from a specific contract:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/events?p=1&ps=10&contract=0x049d36..."
```

---

## Get Events for a Transaction

```
GET /events?txnHash={transactionHash}
```

Returns all events emitted within a specific transaction.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/events?txnHash=0x04a3c..."
```

---

## Response Fields

| Field | Type | Description |
|---|---|---|
| `transactionHash` | string | Hash of the containing transaction |
| `blockNumber` | integer | Block containing the event |
| `contractAddress` | string | Address of the contract that emitted the event |
| `name` | string | Event name (e.g., `Transfer`, `Approval`, `Swap`) |
| `keys` | array | Indexed event keys (array of felt strings) |
| `data` | array | Non-indexed event data (array of felt strings) |
| `timestamp` | integer | Unix timestamp of the containing block |

---

## Understanding Keys and Data

Starknet events consist of two parts:

### Keys

- The first key is typically the **event selector** (a hash of the event name)
- Additional keys are **indexed parameters** that can be used for filtering
- Keys are returned as raw felt values (hex strings)

### Data

- Contains the **non-indexed parameters** of the event
- Order matches the event definition in the contract ABI
- Values are returned as raw felt values (hex strings)

### Decoding Events

To decode event keys and data into meaningful values:

1. Obtain the contract's ABI (from the class declaration or source code)
2. Match the event selector (first key) to the ABI event definition
3. Decode remaining keys and data fields according to their declared types

---

## Common Event Names

| Event Name | Typical Source | Description |
|---|---|---|
| `Transfer` | ERC20 / ERC721 | Token transfer between addresses |
| `Approval` | ERC20 / ERC721 | Spending allowance granted |
| `Swap` | AMM / DEX | Token swap executed |
| `Deposit` | Lending / Bridge | Funds deposited into a protocol |
| `Withdrawal` | Lending / Bridge | Funds withdrawn from a protocol |

---

## Notes

- Events are immutable records of contract activity — they cannot be modified after emission
- A single transaction can emit multiple events from different contracts
- Use the `contract` filter to focus on a single contract's activity
- For token-specific transfer events, the `tokens.md` endpoint provides a more structured interface

---

## Related Files

- `tokens.md` — Structured token transfer queries (built on Transfer events)
- `transactions.md` — Transaction data and per-transaction event lookup
- `contracts.md` — Contract metadata and ABI information
- `pagination.md` — Pagination parameters and iteration strategy
