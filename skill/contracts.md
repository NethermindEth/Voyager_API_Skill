# Contracts Endpoint

## Overview

The contracts endpoint provides access to deployed Starknet contract metadata. You can look up individual contracts by address or query contract classes by class hash.

Starknet separates contract **instances** (deployed at an address) from contract **classes** (reusable code identified by a class hash). Multiple contracts can share the same class.

---

## Get Contract Details

```
GET /contracts/{contractAddress}
```

Returns metadata and details for a specific deployed contract.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts/0x049d36..."
```

### Contract Fields

| Field | Type | Required |Description |
|---|---|---|
| `address` | string | Yes | Contract address |

---

## Get Contract Class

```
GET /classes/{classHash}
```

Returns details about a contract class (the reusable code definition).

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes/0x03e327..."
```

### Class Fields

| Field | Type | Description |
|---|---|---|
| `classHash` | string | Unique class identifier |
| `type` | string | `SIERRA` (Cairo 1+) or `LEGACY` (Cairo 0) |
| `declaredAt` | integer | Block number of the class declaration |
| `declareTransaction` | string | Hash of the declare transaction |

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
