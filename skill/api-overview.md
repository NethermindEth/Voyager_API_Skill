# API Overview

## Base URL

```
https://api.voyager.online/beta
```

All endpoints in this skill are relative to this base URL.

---

## What is Voyager?

Voyager is a **Starknet blockchain indexer API**. It continuously indexes onchain data from the Starknet L2 network and exposes it through a structured REST API optimized for lookups, analytics, monitoring, and application development.

### Voyager vs. RPC Nodes

| | Voyager API | Starknet RPC Node |
|---|---|---|
| **Purpose** | Query indexed, structured data | Submit transactions, read raw state |
| **Data** | Pre-processed, enriched, paginated | Raw blockchain state |
| **Speed** | Fast lookups and filtered queries | Direct chain access |
| **Use case** | Dashboards, analytics, explorers | Wallets, dApps, contract interaction |

Voyager is **not** an RPC node. You cannot submit transactions or execute contract calls through Voyager. Use it to read indexed chain data, inspect contract metadata, track events and messages, and retrieve network statistics.

---

## Available Endpoints

| Endpoint | Description | Reference |
|---|---|---|
| `GET /api-status` | Retrieve current API health and freshness status | `status.md` |
| `GET /blocks` | List Starknet blocks | `blocks.md` |
| `GET /blocks/{blockHash}` | Retrieve block details by hash | `blocks.md` |
| `GET /txns` | List Starknet transactions with filters | `transactions.md` |
| `GET /txns/{txnHash}` | Retrieve transaction details by hash | `transactions.md` |
| `GET /meta-txns` | List meta-transactions for an account contract | `transactions.md` |
| `GET /contracts` | List deployed contracts | `contracts.md` |
| `GET /contracts/{contractAddress}` | Retrieve deployed contract details | `contracts.md` |
| `GET /contracts/{contractAddress}/transfers` | Retrieve token transfers for a contract | `contracts.md` |
| `GET /contracts/{contractAddress}/token-balances` | Retrieve current token balances for a contract | `contracts.md` |
| `GET /contracts/{contractAddress}/erc20-balance-history` | Retrieve historical ERC20 balances for a contract | `contracts.md` |
| `GET /contracts/{contractAddress}/erc20-tokens-held` | Retrieve ERC20 tokens ever held by a contract | `contracts.md` |
| `GET /classes` | List declared contract classes | `classes.md` |
| `GET /classes/{classHash}` | Retrieve class details by class hash | `classes.md` |
| `GET /classes/{classHash}/contracts` | List contracts deployed from a class | `classes.md` |
| `GET /classes/{classHash}/verified` | Check class verification status | `classes.md` |
| `GET /classes/{classHash}/source-code` | Retrieve verified class source code | `classes.md` |
| `GET /tokens` | List deployed tokens with filtering and sorting | `tokens.md` |
| `GET /tokens/{address}/holders` | List holders of a specific token | `tokens.md` |
| `GET /event-activity` | Query token and transfer-related activity | `tokens.md` |
| `GET /events` | Retrieve contract-emitted events | `events.md` |
| `GET /message/{messageHash}` | Retrieve a specific message by hash | `messages.md` |
| `GET /messages` | List Starknet messages | `messages.md` |
| `GET /stats` | Retrieve current Starknet network statistics | `statistics.md` |
| `GET /daily-stats/today` | Retrieve real-time daily metrics | `statistics.md` |
| `GET /daily-stats` | Retrieve historical daily metrics | `statistics.md` |
| `GET /nft-contract/{contract_address}` | Retrieve NFT contract details | `nfts.md` |
| `GET /nft/{contract_address}/{token_id}` | Retrieve NFT token details | `nfts.md` |
| `GET /nft-events` | List NFT events | `nfts.md` |
| `GET /nft-balances` | Retrieve NFT balances for a contract | `nfts.md` |
| `GET /nft-contract-balance` | Retrieve NFT contract balances for an owner | `nfts.md` |
| `GET /nft-holders` | List holders for an NFT contract | `nfts.md` |
| `GET /nft-items` | List NFT items for a contract or owner | `nfts.md` |
| `GET /nft-transfers` | Retrieve NFT transfer history | `nfts.md` |
| `POST /nft/update-metadata/{contract_address}/{token_id}` | Trigger an NFT metadata refresh | `nfts.md` |

---

## Request Format

Most requests are standard **HTTP GET** requests. The NFT metadata refresh endpoint uses **HTTP POST**.

### Required Header

Every request must include the API key header:

```
x-api-key: YOUR_API_KEY
```

See `authentication.md` for full details on obtaining and securing your API key.

### Query Parameters

Voyager currently uses two pagination styles:

- Page-based pagination:
  - `p` — Page number (starts at 1)
  - `ps` — Page size
- Cursor-based pagination:
  - `cursor` — Cursor for the next or previous slice of results
  - `limit` — Number of items to return

Many endpoints also support resource-specific filters such as `block`, `type`, `contract`, `txnHash`, `from_timestamp`, `to_timestamp`, and `hours`. See the endpoint-specific reference files for the exact parameter sets.

---

## Response Format

All responses are returned as **JSON**.

### Successful Responses

- HTTP `200 OK` — Request succeeded; body contains the requested data

### Error Responses

- HTTP `400 Bad Request` — Invalid query parameters
- HTTP `401 Unauthorized` — Missing or invalid API key
- HTTP `404 Not Found` — Resource not found or unavailable for the requested identifier
- HTTP `429 Too Many Requests` — Rate limit exceeded

See `common-errors.md` for detailed troubleshooting guidance.

---

## Data Characteristics

- **Eventually consistent** — Indexed data may have a slight delay behind the chain tip. The most recently produced blocks may not appear instantly.
- **Starknet L2 only** — All data originates from the Starknet Layer 2 network.
- **Mixed pagination models** — Some endpoints use `p` and `ps`, while others use `cursor` and `limit`. See `pagination.md` before building generic iterators.
- **Raw values** — Numeric fields such as token amounts and felts are returned as raw integers or hex values. Apply decimals and ABI-aware decoding where needed.
- **Resource-specific views** — Voyager exposes both generalized data feeds like `events` and higher-level indexed views like tokens, messages, stats, and NFT-specific endpoints.

---

## Related Files

- `authentication.md` — API key setup and security
- `status.md` — API health and freshness
- `blocks.md` — Block endpoints and block fields
- `transactions.md` — Transaction and meta-transaction endpoints
- `contracts.md` — Contract metadata, balances, and transfers
- `classes.md` — Class endpoints, verification, and source code
- `tokens.md` — Token listing, holders, and event activity
- `events.md` — Contract-emitted events
- `messages.md` — L1/L2 message endpoints
- `statistics.md` — Network statistics and daily metrics
- `nfts.md` — NFT contracts, items, holders, balances, and metadata refresh
- `pagination.md` — Pagination parameters and strategies
- `rate-limits.md` — Rate limit policies and retry guidance
- `common-errors.md` — HTTP error codes and troubleshooting
- `best-practices.md` — Production integration guidance
