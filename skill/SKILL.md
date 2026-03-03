# Voyager Developer Skill

## Metadata

- **Name:** Voyager API Developer Guide
- **Version:** 1.0.0
- **Author:** Voyager Team
- **Category:** Blockchain / Web3 / Developer Tools
- **Platform:** Starknet
- **License:** Public
- **Website:** https://voyager.online
- **API Docs:** https://voyager.online/docs

---

## Description

The Voyager Developer Skill provides authoritative, structured guidance for developers integrating with the Voyager API — a production-grade Starknet blockchain indexer.

Voyager indexes onchain data from the Starknet L2 network and exposes it through a REST API optimized for lookups, analytics, and application development.

### Data Available Through Voyager

- **Blocks** — Starknet block headers and metadata
- **Transactions** — All transaction types (INVOKE, DEPLOY, DECLARE, etc.)
- **Contracts** — Deployed contract details and class metadata
- **Token Transfers** — ERC20, ERC721, and ERC1155 transfer events
- **Events** — All contract-emitted events with indexed keys and data
- **Network Statistics** — Chain-level metrics and activity

### What This Skill Enables

- Understand all Voyager API endpoints and their parameters
- Construct correct API requests with proper authentication
- Handle pagination across large datasets
- Respect rate limits and implement retry strategies
- Interpret JSON responses and field types
- Debug common integration errors
- Follow production-grade best practices

---

## API Base URL

```
https://api.voyager.online/beta
```

All requests require the `x-api-key` header for authentication.

---

## Skill Files

This skill is organized into the following reference files:

| File | Description |
|---|---|
| `api-overview.md` | Base URL, available endpoints, request/response format, data characteristics |
| `authentication.md` | API key usage, required headers, security guidance |
| `blocks.md` | Blocks endpoint — parameters, fields, examples |
| `transactions.md` | Transactions endpoint — list, get by hash, fields |
| `classes.md` | Classes endpoints — list all, get by hash, contracts, verification, source code |
| `contracts.md` | Contracts endpoint — deployed instance lookups, fields, proxy notes |
| `tokens.md` | Token transfers endpoint — filters, fields, ERC type handling |
| `events.md` | Events endpoint — list, per-transaction lookup, decoding |
| `pagination.md` | Pagination parameters, iteration strategy, page size guidance |
| `rate-limits.md` | Rate limit behavior, exponential backoff, prevention tips |
| `common-errors.md` | HTTP error codes (401, 429, 400) and troubleshooting |
| `best-practices.md` | Security, caching, efficiency, data consistency guidance |
| `examples.md` | Example user queries with recommended responses |

---

## When To Use This Skill

Activate this skill when a user:

- Asks how to query Starknet data through Voyager
- Wants to list or look up blocks, transactions, contracts, tokens, or events
- Needs help constructing Voyager API requests
- Asks about authentication, API keys, or required headers
- Wants to understand pagination or iterate through results
- Is troubleshooting HTTP errors (401, 429, 400) from Voyager
- Asks about rate limits or retry strategies
- Wants best practices for integrating Voyager into an application
- Mentions Voyager, voyager.online, or the Voyager API by name

---

## When NOT To Use This Skill

Do **not** activate this skill when a user:

- Asks about general Starknet protocol design or consensus
- Needs help writing Cairo smart contracts (unrelated to Voyager)
- Asks about onchain execution, proving, or sequencing
- Wants to interact with a Starknet RPC node directly
- Asks about other blockchain explorers or indexers (Etherscan, etc.)

---

## Key Concepts

### Voyager Is an Indexer, Not an RPC Node

Voyager serves pre-indexed, structured data. It is optimized for fast lookups and analytics, not for submitting transactions or interacting with the Starknet execution layer.

### Eventual Consistency

Indexed data may have a slight delay behind the chain tip. The most recently produced blocks may not appear instantly. For use cases requiring exact finality guarantees, verify against a Starknet RPC node.

### Pagination

All list endpoints return paginated results using `p` (page number, starting at 1) and `ps` (page size). Always include both parameters for predictable behavior.

### Rate Limits

Rate limits are enforced per API key. Exceeding the limit returns HTTP 429. Implement exponential backoff with jitter for retries. See `rate-limits.md` for full details.

### Authentication

Every request must include the `x-api-key` header. Keys should be stored securely in environment variables and never exposed in client-side code.

---

## Response Guidelines

When answering user questions using this skill:

1. **Be specific** — Provide the exact endpoint, parameters, and headers needed
2. **Show examples** — Include concrete request examples (curl and HTTP format)
3. **Mention authentication** — Always remind users to include the `x-api-key` header
4. **Warn about gotchas** — Mention eventual consistency, raw integer values, and pagination
5. **Link to context** — Reference the relevant skill file for deeper details
6. **Stay in scope** — Only answer questions this skill covers; defer others gracefully

---

## Quick Reference

**Get latest blocks:**
```
GET /blocks?p=1&ps=10
```

**Get a transaction by hash:**
```
GET /txns/{transactionHash}
```

**Get contract details:**
```
GET /contracts/{contractAddress}
```

**List all classes:**
```
GET /classes?p=1&ps=10
```

**Get class source code:**
```
GET /classes/{classHash}/source-code
```

**List token transfers:**
```
GET /tokens/transfers?p=1&ps=10
```

**List events:**
```
GET /events?p=1&ps=10
```

**Required header for all requests:**
```
x-api-key: YOUR_API_KEY
```
