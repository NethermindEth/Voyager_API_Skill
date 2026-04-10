# Status Endpoint

## Overview
Retrieve the current status of Voyager APIs, including uptime and lag information.
---

## Get API Status

Retrieve the current status of Voyager APIs, including uptime and lag information.


```
GET /api-status
```


### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/api-status"
```

Note: API Health and Data Freshness
lagSeconds represents how delayed an API is compared to real time.
It is the time difference (in seconds) between the current time and the timestamp of the latest block processed by the API.

status provides a high-level health indicator based on this delay:

ok — less than 1 minute behind real time.
lagging — between 1 and 5 minutes behind.
down — more than 5 minutes behind and considered unhealthy.
These fields give a quick and intuitive view of API freshness and reliability.