# Messages Endpoint

## Overview

Retrieve detailed information about a specific message using its message hash.

---

## Get Message by Hash

```
GET /message/{messageHash}
```

### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `messageHash` | string | Yes | The hash of the message |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/message/${messageHash}"
```

---

## Get All Messages

```
GET /messages
```

Retrieve a paginated list of all messages on Starknet, including L1-L2 and L2-L1 bridge transactions.


### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `cursor` | string | No | Pagination cursor for navigating through results |
| `limit` | integer | No | Number of messages to retrieve per page |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/messages?limit=10"
```

---