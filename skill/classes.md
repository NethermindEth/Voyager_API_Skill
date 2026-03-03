# Classes Endpoint

## Overview

The classes endpoint provides access to Starknet contract classes (reusable smart contract templates). On Starknet, a **class** is a compiled contract definition identified by a class hash, while a **contract** is a deployed instance of a class at a specific address.

You can list all classes, look up a class by hash, find all contracts deployed from a class, check verification status, and retrieve verified source code.

**API Docs:**
- https://voyager.online/docs/api/classes/get-all
- https://voyager.online/docs/api/classes/get-by-hash
- https://voyager.online/docs/api/classes/get-contracts
- https://voyager.online/docs/api/classes/get-verified
- https://voyager.online/docs/api/classes/get-source-code

---

## List All Classes

```
GET /classes
```

Returns a paginated list of all declared contract classes on the Starknet network.

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `p` | integer | Yes | Page number (starts at 1) |
| `ps` | integer | Yes | Page size (items per page) |
| `type` | string | No | Filter by class type: `SIERRA` or `LEGACY` |
| `is_account` | boolean | No | Filter for account classes only |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes?p=1&ps=10"
```

### Filtered Example

List only Sierra (Cairo 1+) classes:

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes?p=1&ps=10&type=SIERRA"
```

---

## Get Class by Hash

```
GET /classes/{classHash}
```

Returns details for a specific contract class identified by its class hash.

### Path Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `classHash` | string | Yes | The class hash (hex string prefixed with `0x`) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes/0x03e327de1c40540b98d05cbcb13552008e36f0ec8d61d46956d2f9752c294328"
```

### Response Fields

| Field | Type | Description |
|---|---|---|
| `hash` | string | Unique class hash |
| `type` | string | `SIERRA` (Cairo 1+) or `LEGACY` (Cairo 0) |
| `declaredBy` | string | Address of the account that declared the class |
| `declaredAtTransaction` | string | Hash of the declare transaction |
| `declaredAt` | integer | Block number when the class was declared |
| `timestamp` | integer | Unix timestamp of the declaration block |
| `version` | integer | Declare transaction version |
| `isAccount` | boolean | Whether this class implements an account contract |
| `numberOfContracts` | integer | Number of deployed contract instances using this class |

---

## Get Contracts by Class

```
GET /classes/{classHash}/contracts
```

Returns a paginated list of all contract instances deployed from a specific class.

### Path Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `classHash` | string | Yes | The class hash (hex string prefixed with `0x`) |

### Query Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `p` | integer | Yes | Page number (starts at 1) |
| `ps` | integer | Yes | Page size (items per page) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes/0x03e327.../contracts?p=1&ps=10"
```

### Response

Returns a list of contract addresses deployed from the specified class. Each item includes the contract address and deployment details.

### Use Cases

- Find all deployments of a known contract template (e.g., all wallets using a specific account class)
- Audit how widely a particular class has been adopted
- Track proxy implementations across the network

---

## Get Verification Status

```
GET /classes/{classHash}/verified
```

Returns the verification status of a class. A verified class has had its source code submitted and matched against the onchain compiled artifact.

### Path Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `classHash` | string | Yes | The class hash (hex string prefixed with `0x`) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes/0x03e327.../verified"
```

### Response Fields

| Field | Type | Description |
|---|---|---|
| `verified` | boolean | Whether the class source code has been verified |
| `verifiedAt` | integer/null | Timestamp of verification (null if not verified) |

### Notes

- Verification means the source code has been compiled and the output matches the onchain class hash
- Unverified classes return `verified: false` — this does not mean the class is malicious, only that source code has not been submitted
- Verification is voluntary and performed by the class declarer or community

---

## Get Source Code

```
GET /classes/{classHash}/source-code
```

Returns the verified source code for a class. Only available for classes that have been verified.

### Path Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `classHash` | string | Yes | The class hash (hex string prefixed with `0x`) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes/0x03e327.../source-code"
```

### Response Fields

| Field | Type | Description |
|---|---|---|
| `name` | string | Contract/class name |
| `sourceCode` | object | Source code files (filename → content mapping) |
| `abi` | array | Contract ABI definition |
| `compilerVersion` | string | Cairo compiler version used |
| `license` | string/null | License identifier (e.g., `MIT`, `Apache-2.0`) |

### Notes

- Returns HTTP `404` if the class has not been verified
- Source code is returned as a mapping of filenames to their content
- The ABI is useful for decoding events, function calls, and constructor parameters
- Compiler version helps reproduce the build for auditing purposes

---

## Class Types

| Type | Description |
|---|---|
| `SIERRA` | Cairo 1+ compiled class (current standard). Sierra is the intermediate representation that gets compiled to CASM for execution. |
| `LEGACY` | Cairo 0 compiled class (deprecated). These are raw CASM classes from before the Sierra compilation pipeline. |

---

## Common Patterns

### Check if a class is verified and fetch source code

```python
import requests

headers = {"x-api-key": "YOUR_API_KEY"}
base_url = "https://api.voyager.online/beta"
class_hash = "0x03e327..."

# Step 1: Check verification status
verified_resp = requests.get(
    f"{base_url}/classes/{class_hash}/verified",
    headers=headers
)
is_verified = verified_resp.json().get("verified", False)

# Step 2: Fetch source code if verified
if is_verified:
    source_resp = requests.get(
        f"{base_url}/classes/{class_hash}/source-code",
        headers=headers
    )
    source = source_resp.json()
    print(f"Contract: {source['name']}")
    print(f"Compiler: {source['compilerVersion']}")
else:
    print("Class is not verified — source code unavailable")
```

### List all contracts deployed from a class

```python
import requests

headers = {"x-api-key": "YOUR_API_KEY"}
base_url = "https://api.voyager.online/beta"
class_hash = "0x03e327..."

page = 1
all_contracts = []

while True:
    response = requests.get(
        f"{base_url}/classes/{class_hash}/contracts",
        params={"p": page, "ps": 25},
        headers=headers
    )
    items = response.json().get("items", [])
    all_contracts.extend(items)

    if len(items) < 25:
        break
    page += 1

print(f"Found {len(all_contracts)} contracts using class {class_hash}")
```

---

## Notes

- Class hashes are hex strings prefixed with `0x`
- A single class can be deployed as many contract instances
- The `/classes` list endpoint is paginated — use `p` and `ps` parameters
- Source code is only available for verified classes
- The ABI from verified source code is essential for decoding events and transactions
- Use `/classes/{classHash}/contracts` to understand the adoption footprint of a specific implementation

---

## Related Files

- `contracts.md` — Deployed contract instance lookups
- `transactions.md` — Declare transactions that register new classes
- `events.md` — Events emitted by contracts (decode using the class ABI)
- `pagination.md` — Pagination parameters and iteration strategy
- `api-overview.md` — Base URL and request format
