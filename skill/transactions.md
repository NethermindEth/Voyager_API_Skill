# Transactions Endpoint

## Overview

The transactions endpoint provides access to Starknet transaction data. You can list transactions with pagination and filters, or retrieve a specific transaction by its hash.

---

## List Transactions

```
GET /txns
```

Returns a paginated list of Starknet transactions, ordered from most recent to oldest.

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `to` | string | No | Only transactions from this contract address will be retrieved |
| `rejected` | boolean | No | f true, only rejected transactions will be retrieved. Otherwise, only non-rejected transactions |
| `p` | integer | Yes | Page number (starts at 1) |
| `ps` | integer | Yes | Page size (items per page) |
| `block` | integer | No | Filter by block number |
| `type` | string | No | Filter by transaction type (INVOKE, DEPLOY, DECLARE, etc.) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/txns?p=1&ps=10"
```

### Filtered Example

Fetch only INVOKE transactions from block 650000:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/txns?p=1&ps=10&block=650000&type=INVOKE"
```

---

## Get Transaction by Hash

```
GET /txns/{txnHash}
```

Returns full details for a specific transaction identified by its hash.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/txns/0x04a3c..."
```

---

## Get Meta-Transactions

```
GET /txns/{meta-txns}
```

List meta-transactions (transactions using the "execute from outside" pattern) for a specific contract account.

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `to` | string | Yes | Target contract address (hex string) - the account contract receiving the meta-transaction |
| `p` | integer | No | Page number (starts at 1) |
| `ps` | integer | No | Page size (items per page) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/meta-txns?to=0x04af........c7fa9b&p=1&ps=10"
```

---

## Response Fields

| Field | Type | Description |
|---|---|---|
| `hash` | string | Unique transaction hash |
| `blockNumber` | integer | Block containing this transaction |
| `type` | string | Transaction type: `INVOKE`, `DEPLOY`, `DEPLOY_ACCOUNT`, `DECLARE`, `L1_HANDLER` |
| `status` | string | Execution status: `ACCEPTED_ON_L2`, `ACCEPTED_ON_L1`, `REJECTED` |
| `timestamp` | integer | Unix timestamp of the containing block |
| `sender` | string | Sender account address |
| `actualFee` | string | Actual fee paid (raw integer, in wei) |
| `nonce` | integer | Transaction nonce |
| `version` | string | Transaction version |

---

## Transaction Types

| Type | Description |
|---|---|
| `INVOKE` | Standard function call (most common) |
| `DEPLOY` | Legacy contract deployment |
| `DEPLOY_ACCOUNT` | Account contract deployment |
| `DECLARE` | Contract class declaration |
| `L1_HANDLER` | Message from Ethereum L1 to Starknet L2 |

---

## Notes

- Transaction hashes are hex strings prefixed with `0x`
- The `fee` field is returned as a raw integer string; divide by 10^18 for ETH
- Transactions are returned in reverse chronological order when listing
- Use the `block` filter to scope results to a specific block
- Use the `type` filter to focus on a specific transaction category

---

## Related Files

- `blocks.md` — Block data and block-level queries
- `events.md` — Events emitted by transactions
- `contracts.md` — Contract details referenced in transactions
- `pagination.md` — Pagination parameters and iteration strategy
