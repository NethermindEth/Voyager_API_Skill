# NFT APIs Endpoint

---

## Get NFT Contract

```
GET /nft-contract/{contract_address}
```

Retrieve detailed information about an NFT contract including its metadata, statistics, and collection information.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/contracts?p=1&ps=10"
```

### Contract Fields

| Field | Type | Required |Description |
|---|---|---|
| `contract_address` | string | Yes | The NFT contract address

---

## Get NFT Details

```
GET /nft/{contract_address}/{token_id}
```

Retrieve detailed information about a specific NFT token by its contract address and token ID.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/nft/${contractAddress}/${tokenId}"
```

### Contract Fields

| Field | Type | Required |Description |
|---|---|---|
| `contract_address` | string | Yes | The NFT contract address |
| `token_id` | string | Yes | The unique token ID within the contract |

---

## Get NFT Events

```
GET /nft-events
```

Retrieve events related to NFT contracts including mints, transfers, and burns with cursor-based pagination.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/nft-events?contract_address=0x01435498bf393da86b4733b9264a86b58a42b31f8d8b8ba309593e5c17847672&limit=10"
```


### Query Parameters

| Field | Type | Required |Description |
|---|---|---|
| `contractAddress` | string | Yes | The contract address to retrieve transfers for
| `token_id` | string | No | Filter events for a specific token ID
| `cursor` | string | No | Pagination cursor for next/previous page
| `limit` | integer | No | 	Number of events to return (default: 50, max: 100)

---

## Get NFT Balances

```
GET /nft-balances
```

Retrieve NFT token balances for a specific contract, optionally filtered by token ID.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/nft-balances?contract_address=0x01435498bf393da86b4733b9264a86b58a42b31f8d8b8ba309593e5c17847672&p=1&ps=10"
```

### Query Parameters

| Field | Type | Required |Description |
|---|---|---|
| `contractAddress` | string | Yes | The NFT contract address |
| `token_id` | string | No | Filter balances for a specific token ID
| `p` | integer | No | Page number (default: 1)
| `ps` | integer | No | 	Page size (default: 10)

---

## Get NFT Contract Balance

```
GET /nft-contract-balance
```

Retrieve the NFT contract balances for a specific owner address, showing how many tokens they own from each contract.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/nft-contract-balance?owner_address=${ownerAddress}&limit=10"
```

### Query Parameters

| Field | Type | Required |Description |
|---|---|---|
| `owner_address` | string | Yes | The owner address to query balances for |
| `contract_address` | string | No | The NFT contract address |
| `cursor` | string | No | Pagination cursor for next/previous page |
| `limit` | integer | No | Number of items to return (default: 50, max: 100) |

---

## Get NFT Holders

```
GET /nft-holders
```

Retrieve a list of addresses holding tokens from a specific NFT contract, with optional filtering by token ID.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/nft-holders?contract_address=0x01435498bf393da86b4733b9264a86b58a42b31f8d8b8ba309593e5c17847672&p=1&ps=10"
```

### Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `contractAddress` | string | Yes | The NFT contract address |
| `token_id` | string | No | Filter events for a specific token ID
| `p` | integer | No | Page number (default: 1)
| `ps` | integer | No | 	Page size (default: 10)
---

## Get NFT Items

```
GET /nft-items
```

Retrieve a list of NFT items from a contract, optionally filtered by owner address, with cursor-based pagination.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/nft-items?contract_address=0x01435498bf393da86b4733b9264a86b58a42b31f8d8b8ba309593e5c17847672&limit=10"
```

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `owner_address` | string | No | Filter items owned by a specific address |
| `contract_address` | string | Yes | The NFT contract address |
| `cursor` | string | No | Pagination cursor for next/previous page |
| `limit` | integer | No | Number of items to return (default: 50, max: 100) |

---

## Get NFT Transfers

```
GET /nft-items
```

Retrieve transfer history for owner's contract address with filtering by NFT contract type, and cursor-based pagination.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/nft-transfers?contract_address=0x01435498bf393da86b4733b9264a86b58a42b31f8d8b8ba309593e5c17847672&type=erc721&limit=10"
```

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `type` | string | No | NFT standard ("erc721", "erc1155", default: "erc721") |
| `contract_address` | string | Yes | The account address |
| `cursor` | string | No | Pagination cursor for next/previous page |
| `limit` | integer | No | Number of items to return (default: 50, max: 100) |

---

## Update NFT Metadata

```
POST /nft/update-metadata/{contract_address}/{token_id}
```

Trigger a metadata update for a specific NFT token by its contract address and token ID.

Note: Rate Limit
Each token can only be updated once per hour.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/nft/update-metadata/${contractAddress}/${tokenId}"
```

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `contract_address` | string | Yes | The NFT contract address |
| `token_id` | string | No | The unique token ID within the contract |
