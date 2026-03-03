# Tokens Endpoint

## Overview

The tokens endpoint provides access to token transfer events on Starknet. You can query ERC20, ERC721, and ERC1155 transfers with optional filters by contract, sender, or recipient.

---

## List Token Transfers

```
GET /tokens/transfers
```

Returns a paginated list of token transfer events, ordered from most recent to oldest.

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `p` | integer | Yes | Page number (starts at 1) |
| `ps` | integer | Yes | Page size (items per page) |
| `contract` | string | No | Filter by token contract address |
| `from` | string | No | Filter by sender address |
| `to` | string | No | Filter by recipient address |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/tokens/transfers?p=1&ps=10"
```

### Filtered Examples

Transfers for a specific token contract:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/tokens/transfers?p=1&ps=10&contract=0x049d36..."
```

Transfers sent by a specific address:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/tokens/transfers?p=1&ps=10&from=0x01a2b3..."
```

---

## Response Fields

| Field | Type | Description |
|---|---|---|
| `transactionHash` | string | Hash of the containing transaction |
| `from` | string | Sender address |
| `to` | string | Recipient address |
| `value` | string | Amount transferred (raw integer, unscaled) |
| `tokenId` | string | Token ID (ERC721 and ERC1155 only) |
| `tokenAddress` | string | Token contract address |
| `tokenType` | string | `ERC20`, `ERC721`, or `ERC1155` |
| `timestamp` | integer | Unix timestamp of the containing block |
| `blockNumber` | integer | Block containing the transfer |

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

## Notes

- Combine `from` and `to` filters to find transfers between two specific addresses
- The `contract` filter is the most efficient way to narrow results to a single token
- Transfers are indexed from emitted `Transfer` events, not from transaction calldata
- Mint events typically have `from` set to the zero address (`0x0`)
- Burn events typically have `to` set to the zero address (`0x0`)

---

## Related Files

- `contracts.md` — Token contract details and class information
- `events.md` — Raw Transfer events and other contract events
- `transactions.md` — Transactions containing token transfers
- `pagination.md` — Pagination parameters and iteration strategy
