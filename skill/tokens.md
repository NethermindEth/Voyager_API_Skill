# Tokens Endpoint

## Overview

List all deployed tokens on the Starknet network with filtering and sorting options.

---

## Get All Tokens

```
GET /tokens
```

List all deployed tokens on the Starknet network with filtering and sorting options.

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `type` | string | No | Token type (default: "erc20"). Allowed: erc20, erc721, erc1155 |
| `attribute` | string | No | Order by specific attribute (default: "holders"). Allowed: holders, transfers |
| `p` | integer | No | Page number to retrieve (starts from 1) |
| `ps` | integer | No | The number of items to return in a page. |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/tokens?type=erc20&attribute=holders&p=1&ps=10"
```
---


## Get Token Holders

```
GET /tokens/{address}/holders
```

Retrieves a paginated list of token holders for a specific ERC20 token, including holder addresses, balances, aliases, and last transfer times.

### Path Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `address` | string | Yes | Token contract address |


### Query Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `p` | integer | No | Page number to retrieve (starts from 1) |
| `ps` | integer | No | The number of items to return in a page. |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/tokens/0x053c91253bc9682c04929ca02ed00b3e423f6710d2ee7e0d5ebb06f3ecf368a8/holders?ps=10&p=1"
```

---
## Get Event Activities

```
GET /event-activity
```

List event activities such as token transfers with detailed filtering options. For contract-specific transfers, see Get Contract Transfers.

### Query Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `token_type` | string | No | Token standards filter (e.g., "erc20", "erc721", "erc1155") |
| `from_block` | integer | No | Starting block number for the query |
| `to_block` | integer | No | Ending block number for the query |
| `from_timestamp` | integer | No | Lower time boundary (seconds since Unix epoch) |
| `to_timestamp` | integer | No | Upper time boundary (seconds since Unix epoch) |
| `from_address` | string | No | Address sending tokens |
| `to_address` | string | No | Address receiving tokens |
| `sort` | string | No | Order of results ("asc" or "desc") |
| `page_size` | integer | No | Number of items per page |
| `last_id` | string | No | ID of the last transfer for pagination |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/event-activity?token_type=erc20&page_size=10&sort=desc"
```

---

## Token Types

| Type | Value Field | Description |
|---|---|---|
| `ERC20` | `value` (amount) | Fungible token transfer — `value` is a raw integer; apply the token's `decimals` to get the human-readable amount |
| `ERC721` | `tokenId` | Non-fungible token transfer — each token is unique, identified by `tokenId` |
| `ERC1155` | `value` + `tokenId` | Semi-fungible token transfer — includes both a `tokenId` and a `value` (quantity) |

---

## Working with Token Values

Token `value` fields are returned as **raw integers** (no decimal scaling applied).

To convert to a human-readable amount for ERC20 tokens:

```
human_readable = raw_value / (10 ^ decimals)
```

For example, if `value` is `"1000000000000000000"` and the token has 18 decimals:

```
1000000000000000000 / 10^18 = 1.0 token
```

You can retrieve the token's decimals from the contract metadata or ABI.

---

## Related Files

- `contracts.md` — Token contract details and class information
- `events.md` — Raw Transfer events and other contract events
- `transactions.md` — Transactions containing token transfers
- `pagination.md` — Pagination parameters and iteration strategy
