# Contracts Endpoint

## Overview

The contracts endpoint provides access to deployed Starknet contract metadata. You can look up individual contracts by address or query contract classes by class hash.

Starknet separates contract **instances** (deployed at an address) from contract **classes** (reusable code identified by a class hash). Multiple contracts can share the same class.

---

## Get All Contract

```
GET /contracts
```

Retrieve a paginated list of all contracts deployed on Starknet. Contracts are deployed from classes.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts?p=1&ps=10"
```

### Contract Fields

| Field | Type | Required |Description |
|---|---|---|
| `type` | string | No | Contract type, not specified = all types (types: account, erc20, erc721, erc1155, unknown, proxy)
| `p` | integer | No | Page number to retrieve (starts from 1)
| `ps` | integer | No | 	The number of items to return in a page

---

## Get Contract Details

```
GET /contracts/{contractAddress}
```

Returns metadata and details for a specific deployed contract.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts/${contractAddress}"
```

### Contract Fields

| Field | Type | Required |Description |
|---|---|---|
| `address` | string | Yes | Contract address |

---

## Get Contract Transfers

```
GET /contracts/{contractAddress}/transfers
```

Retrieve ERC20, ERC721, or ERC1155 token transfers associated with a specific contract address.

Note: Important: It's highly recommended to set the timestampFrom and timestampTo parameters, and to keep them within three calendar month time ranges. Each additional quarter queried will result in a noticeable increase in response time.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts/${contractAddress}/transfers?timestampFrom=${timestampFrom}&timestampTo=${timestampTo}&type=erc20&ps=10"
```

### Contract Fields

Path Parameters

| Field | Type | Required |Description |
|---|---|---|
| `contractAddress` | string | Yes | The contract address to retrieve transfers for

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `timestampFrom` | integer | Recommended | Retrieve transfers from this Unix timestamp (seconds since epoch)
| `timestampTo` | integer | Recommended | Retrieve transfers up to this Unix timestamp (seconds since epoch) (exclusive)
| `type` | string | No | Filter by token type. Allowed: erc20, erc721, erc1155 (default: erc20)
| `id` | string | No | Filter transfers before or after a specific transfer ID
| `order_by` | string | No | Results order. Allowed: asc, desc (default: desc)
| `ps` | integer | No | 	Number of items per page (algorithm: less than 25 = 10; 25-49 = 25; 50-99 = 50; 100+ = 100)
| `from` | string | No | Only ERC transfers from the specified address will be retrieved
| `to` | string | No | Only ERC transfers to the specified address will be retrieved
| `symbol` | string | No | Filter by ERC token symbol
| `tokenAddress` | string | No | 	Filter by ERC token contract address
| `invocationType` | string | No | 	Transaction type. Allowed: ALL, VALIDATE, EXECUTE, FEE_TRANSFER (default: ALL)

---

## Get Contract Class

```
GET /classes/{classHash}
```

Returns details about a contract class (the reusable code definition).

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes/{contractAddress}"
```

### Class Fields

| Field | Type | Description |
|---|---|---|
| `classHash` | string | Unique class identifier |
| `type` | string | `SIERRA` (Cairo 1+) or `LEGACY` (Cairo 0) |
| `declaredAt` | integer | Block number of the class declaration |
| `declareTransaction` | string | Hash of the declare transaction |

---

## Get Contract Balances

```
GET /contracts/{contractAddress}/token-balances
```

Retrieve the token balances of a specific contract address.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts/${contractAddress}/token-balances"
```

### Contract Fields

| Field | Type | Required |Description |
|---|---|---|
| `contractAddress` | string | Yes | The contract address to retrieve token balances for |

---

## Get Contract Erc20 Balance History

```
GET /contracts/{contractAddress}/erc20-balance-history
```

Retrieve the historical daily balances of ERC20 tokens ever held by a contract address.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts/${contractAddress}/erc20-balance-history"
```

Path Parameters
| Field | Type | Required |Description |
|---|---|---|
| `contractAddress` | string | Yes | The contract address to retrieve balance history for |

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `timerange` | '1y', '1m', '3m', '6m' | '1w' | No | Time range for the history. Default is '1m'
| `token_address` | string | No | Filter results to a specific ERC20 token address. If not provided, returns balance history for all tokens.
---

## Get Erc20 Tokens Held

```
GET /contracts/{contractAddress}/erc20-tokens-held
```

Retrieve the historical daily balances of ERC20 tokens ever held by a contract address.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts/${contractAddress}/erc20-tokens-held"
```

Path Parameters
| Field | Type | Required |Description |
|---|---|---|
| `contractAddress` | string | Yes | The contract address to retrieve the tokens-ever-held list for |

---

## Contract Types

| Type | Description |
|---|---|
| `ERC20` | Fungible token contract |
| `ERC721` | Non-fungible token (NFT) contract |
| `ACCOUNT` | Account abstraction contract (wallet) |
| `PROXY` | Proxy/upgradeable contract |
| `GENERIC` | Any other contract type |

---

## Class Types

| Type | Description |
|---|---|
| `SIERRA` | Cairo 1+ compiled class (current standard) |
| `LEGACY` | Cairo 0 compiled class (deprecated) |

---

## Notes

- Use the **contract address** to look up a specific deployed instance
- Use the **class hash** to look up shared implementation code
- Proxy contracts have their own class hash, which may differ from the underlying implementation's class hash
- A single class hash can be shared by many deployed contract instances
- All addresses and hashes are hex strings prefixed with `0x`

---

## Related Files

- `classes.md` — Class lookups, verification, source code, and contracts-by-class
- `transactions.md` — Deploy and declare transactions
- `tokens.md` — Token contract transfers
- `events.md` — Events emitted by contracts
- `api-overview.md` — Base URL and request format
