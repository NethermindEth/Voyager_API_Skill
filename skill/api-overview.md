# API Overview

## Base URL

```
https://api.voyager.online/beta
```

All endpoints are prefixed with this base URL.

---

## What is Voyager?

Voyager is a **Starknet blockchain indexer API**. It continuously indexes onchain data from the Starknet L2 network and exposes it through a structured REST API optimized for lookups, analytics, and application development.

### Voyager vs. RPC Nodes

| | Voyager API | Starknet RPC Node |
|---|---|---|
| **Purpose** | Query indexed, structured data | Submit transactions, read raw state |
| **Data** | Pre-processed, enriched, paginated | Raw blockchain state |
| **Speed** | Fast lookups and filtered queries | Direct chain access |
| **Use case** | Dashboards, analytics, explorers | Wallets, dApps, contract interaction |

Voyager is **not** an RPC node. You cannot submit transactions or call contract functions through Voyager. Use it for reading and querying indexed chain data.

---

## Available Endpoints

| Endpoint | Description | Reference |
|---|---|---|
| `GET /blocks` | List and retrieve Starknet blocks | `blocks.md` |
| `GET /txns` | List and retrieve transactions | `transactions.md` |
| `GET /contracts/{address}` | Look up deployed contract details | `contracts.md` |
| `GET /classes/{classHash}` | Look up contract class metadata | `contracts.md` |
| `GET /tokens/transfers` | Query token transfer events | `tokens.md` |
| `GET /events` | Retrieve contract-emitted events | `events.md` |

---

## Request Format

All requests are standard **HTTP GET** requests.

### Required Header

Every request must include the API key header:

```
x-api-key: YOUR_API_KEY
```

See `authentication.md` for full details on obtaining and securing your API key.

### Query Parameters

List endpoints accept pagination parameters:

- `p` — Page number (starts at 1)
- `ps` — Page size (number of items per page)

Many endpoints also support optional filters. See the individual endpoint reference files for available parameters.

---

## Response Format

All responses are returned as **JSON**.

### Successful Responses

- HTTP `200 OK` — Request succeeded; body contains the requested data

### Error Responses

- HTTP `400 Bad Request` — Invalid query parameters
- HTTP `401 Unauthorized` — Missing or invalid API key
- HTTP `429 Too Many Requests` — Rate limit exceeded

See `common-errors.md` for detailed troubleshooting guidance.

---

## Data Characteristics

- **Eventually consistent** — Indexed data may have a slight delay behind the chain tip. The most recently produced blocks may not appear instantly.
- **Starknet L2 only** — All data originates from the Starknet Layer 2 network.
- **Paginated** — All list endpoints return paginated results. See `pagination.md` for iteration strategies.
- **Raw values** — Numeric fields (token amounts, felts) are returned as raw integers. Apply decimals or decoding as needed based on the contract ABI.

---

## Related Files

- `authentication.md` — API key setup and security
- `pagination.md` — Pagination parameters and strategies
- `rate-limits.md` — Rate limit policies and retry guidance
- `common-errors.md` — HTTP error codes and troubleshooting
- `best-practices.md` — Production integration guidance
