# Stats Endpoint

## Overview

Retrieve detailed information about a specific message using its message hash.

---

## Get Stats

Retrieve current network statistics for Starknet, including transaction counts, performance metrics, and financial data.


```
GET /stats
```


### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/stats"
```

---

## Get Today's Stats

```
GET /daily-stats/today
```

Returns real-time aggregated metrics for the Starknet network for the current day, including transaction stats, user operations, network activity, account growth, TVL, and block fees.


### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `hours` | string | No | Time window for stats: 1h, 4h, or 24h (default: 24h) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/daily-stats/today?hours=24h"
```

---

## Get Historical Daily Stats

```
GET /daily-stats
```

Retrieves aggregated historical metrics for the Starknet network over a specified time range. Supports querying individual metrics or all available metrics across different time periods.


### Parameters

| Parameter | Type | Required | Description |
|---|---|---|---|
| `metrics` | string | No | Metric to return (default: * for all metrics) |
| `timerange` | string | No | Time range: max, 1y, 1m, 1w, 1d (default: 1m) |

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/daily-stats?metrics=*&timerange=1w"
```

### Available Metrics

#### Transaction & Operations Metrics
transactions_per_block
transactions_per_second
transactions_count
max_transactions_per_second
user_operations_count
user_operations_per_block
user_operations_per_second
max_user_operations_per_second
Account Call Metrics
account_calls_count
account_calls_per_block
account_calls_per_second
max_account_calls_per_second


#### Network Metrics
classes_count
events_count
messages_count
contracts_count
cairo_1_classes
cairo_1_contracts
account_contracts_count
active_account_contracts


#### Block Metrics
l1_block_creation_time
l2_block_creation_time
proof_generation_time


#### Special Metrics (Structured Data)
account_contracts - Daily and cumulative account contracts by wallet type
active_accounts - Active accounts by wallet type per day
fee_per_block - Fees per block in USD, ETH, and STRK
gas_per_block - Average L1, L1 data, and L2 gas usage per block
tvl - Total value locked by token and protocol
eth_transfer_fee - L1 vs L2 ETH transfer cost comparison
erc20_transfer_fee - L1 vs L2 ERC20 transfer cost comparison
swap_fee - L1 vs L2 swap cost comparison
nft_mint_fee - L1 vs L2 NFT mint cost comparison
starkgate_eth_deposit_fee - Bridge deposit costs
starkgate_eth_withdrawal_fee - Bridge withdrawal costs
l1_block_verification_cost - Average block verification cost on L1


---