# Staking Endpoint

## Overview

Get an overview of staking statistics and metrics for the Starknet network

---

## Get Staking Overview

```
GET /staking/overview
```

Get an overview of staking statistics and metrics for the Starknet network

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/overview"
```

---

## Get Validators

```
GET /staking/validators
```

Returns a paginated list of staking validators with detailed information including stake amounts, delegators, commission rates, APR calculations, and staking power metrics for both STRK and BTC assets.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/validators?p=1&ps=10&sortBy=stakingPower&sortOrder=DESC"
```

### Query Parameters

| Field | Type | Required |Description |
|---|---|---|
| `p` | integer | No | Page number (default: 1) |
| `ps` | integer | No | Page size (default: 25) |
| `search` | string | No | Search term to filter validators by address or name |
| `sortBy` | string | No | Field to sort by: rank, stakeStrk, stakeBtc, delegators, commission, stakingPower (default: rank) |
| `sortOrder` | string | No | Sort direction: ASC or DESC (default: ASC) |
| `tokenAddress` | string | No | Filter validators by staked token address (hex string) |


Notes
1. APR values are in basis points (divide by 100 for percentage, e.g., 1213 = 12.13%)
2. Revenue share (commission) is also in basis points
3. Liveness percentage indicates validator uptime and attestation reliability
4. Staking power combines STRK and BTC stakes with respective weights

---

## Get Validator Details

```
GET /staking/validator-details
```

Returns detailed information about a specific validator, including comprehensive staking metrics, pool information, social links, operational details, liveness statistics, and commitment parameters.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/validator-details?validator=0x00d3b910d8c528bf0216866053c3821ac6c97983dc096bff642e9a3549210ee7"
```

Query Parameters

| Field | Type | Required |Description |
|---|---|---|
| `validator` | string | Yes | The validator's address (hexadecimal string)

---

## Get Validator Pool Info

```
GET /staking/validator-pool-info
```

Returns a list of all staking pools associated with a specific validator, including pool contract addresses and token information for each pool.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/classes/{contractAddress}"
```

### Class Fields

| Field | Type | Required | Description |
|---|---|---|
| `validator` | string | Yes | The validator's address (hexadecimal string) |

Note:
1. Returns an array of pool objects (not paginated)
2. Each validator can have multiple pools for different tokens (STRK, BTC variants, etc.)
3. Useful for understanding which tokens a validator accepts for staking

---

## Get Delegators

```
GET /staking/delegators
```

Returns detailed information about delegators for a specific validator, including delegator addresses, staked amounts, share percentages, and start times. Supports filtering by specific delegator and asset type (STRK or BTC).

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/delegators?validator=0x00d3b910d8c528bf0216866053c3821ac6c97983dc096bff642e9a3549210ee7&p=1&ps=25&asset=strk"
```

### Query Parameters

| Field | Type | Required |Description |
|---|---|---|
| `validator` | string | Yes | Validator's address (hex string) |
| `p` | integer | No | Page number (default: 1)
| `ps` | integer | No | Page size (default: 25)
| `delegator` | integer | No | Specific delegator address (hex string) or Starknet ID (.stark domain). Returns only that delegator without pagination
| `asset` | integer | No | Asset type: strk or btc (default: strk)

Notes
1. When delegator parameter is provided, returns single delegator info without pagination
2. share is in basis points (divide by 100 for percentage)
3. Results are typically sorted by delegatedStake (descending)
4. Supports both STRK and BTC delegations via asset parameter

---

## Get Wallet Info

```
GET /staking/wallet-info
```

Returns comprehensive staking information for a specific wallet address, including an overview of total staked amounts and rewards across all validators, as well as detailed information about each individual staking position.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/wallet-info?address=0x01bed88e5027795facabb1a8fe8fa882f56a734412697232d9e7c86e18589b18"
```

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `address` | string | Yes | The wallet address to query (hexadecimal string) |

Notes
1. All token amounts are returned in the smallest unit (wei for STRK/ETH)
2. To convert to decimal format, divide by 10^decimals (18 for STRK)
3. APR values are in basis points (divide by 100 for percentage)
4. Only active staking positions are returned

---

## Get Wallet Staking Activity

```
GET /staking/wallet-info/activity
```

Returns a paginated list of all staking-related activities for a specific wallet address, including stakes, withdrawals, reward claims, and stake movements. Supports filtering by operation type, destination validator, date range, and search terms.

### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/wallet-info/activity?address=0x01bed88e5027795facabb1a8fe8fa882f56a734412697232d9e7c86e18589b18&p=1&ps=10"
```

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `address` | string | Yes | Wallet address to query activity for (hex string) |
| `p` | integer | No | Page number (default: 1) |
| `ps` | integer | No | Page size (default: 25) |
| `destination` | string | Yes | Filter by destination validator address |
| `sort` | string | No | Sort order by timestamp: ASC or DESC (default: DESC) |
| `start` | string | No | Start date filter (ISO 8601 format, e.g., "2024-01-01") |
| `end` | string | No | End date filter (ISO 8601 format) |
| `operation` | string | No | Filter by operation type (see available types below) |
| `search` | string | No | Search term to filter by destination names or addresses |


Notes
1. Activities are sorted by timestamp (most recent first by default)
2.meta object provides available filter options based on wallet's history
3. Amount values are in wei (divide by 10^decimals for readable format)
4. Transaction hashes can be used to look up full transaction details

---

## Get Validator Activity

```
GET /staking/validator-details/activity
```

Returns a paginated list of all staking-related activities for a specific validator, including validator operations, delegator interactions, reward claims, and withdrawals.


### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/validator-details/activity?address=0x072543946080646d1aac08bb4ba6f6531b2b29ce41ebfe72b8a6506500d5220e&p=1&ps=10"
```

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `address` | string | Yes | Validator address (hex string) |
| `p` | integer | No | Page number (default: 1) |
| `ps` | integer | No | Page size (default: 25) |
| `sort` | string | No | Sort order by timestamp: ASC or DESC (default: DESC) |
| `start` | string | No | Start date filter (ISO 8601 format, e.g., "2024-01-01") |
| `end` | string | No | End date filter (ISO 8601 format) |
| `operation` | string | No | Filter by operation type (see available types below) |
| `search` | string | No | Search by delegator addresses or names |


Notes
1. Includes both validator's own operations and delegator interactions
2. Sorted by timestamp (most recent first by default)
3. Useful for validator dashboards and analytics
4. Large validators may have thousands of pages of activity

---

## Get Delegators Over Time

```
GET /staking/delegators-over-time
```

Returns historical time-series data showing how delegator counts and delegation amounts have changed over time. Can provide data for a specific validator or network-wide statistics.


### Example

Network-wide delegator trends:
```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/delegators-over-time?timerange=1m"
```

Specific validator:
```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/delegators-over-time?address=0x00d3b910d8c528bf0216866053c3821ac6c97983dc096bff642e9a3549210ee7&timerange=1m"
```

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `address` | string | No | Validator address (if omitted, returns network-wide stats) |
| `timerange` | string | No | Time period: max, 1y, 1m, 1w (default: 1m) |


Notes
1. Returns an array of daily data points
2. delegationChange can be negative when delegators leave
3. Network-wide stats aggregate all validators
4. Useful for visualizing delegator growth trends

---

## Get Stake Over Time

```
GET /staking/stake-over-time
```

Returns historical time-series data showing how staking amounts have changed over time, with breakdowns by asset type (STRK/BTC) and stake source (own stake vs delegated stake).


### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/stake-over-time?address=0x00d3b910d8c528bf0216866053c3821ac6c97983dc096bff642e9a3549210ee7&timerange=1w"
```

Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `address` | string | No | Validator address (if omitted, returns network-wide stats) |
| `timerange` | string | No | Time period: max, 1y, 1m, 1w (default: 1m) |


Notes
1. All stake amounts are formatted with decimals
2. Change values can be negative (indicating withdrawals/unstaking)
3. Network-wide mode (without address) shows aggregate data
4. Useful for tracking validator growth and stake composition

---

## Get Wallet Stake Over Time

```
GET /staking/wallet-info/stake-over-time
```

Returns historical time-series data showing how a specific wallet's staking amounts have changed over time across all validators.


### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/wallet-info/stake-over-time?address=0x01bed88e5027795facabb1a8fe8fa882f56a734412697232d9e7c86e18589b18&timerange=1m"
```


Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `address` | string | Yes | Wallet address (hex string) |
| `timerange` | string | No | Time period: max, 1y, 1m, 1w (default: 1m) |


Notes
1. Aggregates stake across all validators where wallet has delegated
2. Change values can be negative (withdrawals/unstaking)
3. Useful for personal portfolio tracking
Returns empty array if wallet has never staked

---

## Get Attestations

```
GET /staking/attestations
```

Returns a paginated list of attestations made by validators in the staking system. Attestations are cryptographic confirmations made by validators at each epoch to prove their liveness and participation in consensus.


### Example

```
curl -H "x-api-key: YOUR_API_KEY" \
     "https://api.voyager.online/beta/staking/attestations?validator=0x00d3b910d8c528bf0216866053c3821ac6c97983dc096bff642e9a3549210ee7&p=1&ps=10"
```


Query Parameters
| Field | Type | Required |Description |
|---|---|---|
| `validator` | string | No | Validator address to filter attestations (if omitted, returns all validators) |
| `p` | integer | No | Page number (default: 1)
| `ps` | integer | No | Page size (default: 25)


What are Attestations?
Attestations are proof that a validator is actively participating in the network:

1. Liveness Proof: Validators must attest each epoch to prove they're online

2. Consensus Participation: Confirms validator's role in block validation

3. Reward Eligibility: Missing attestations can affect rewards

4. Performance Metric: Attestation history shows validator reliability

Use Cases
1. Validator Monitoring: Track if a validator is consistently attesting

2. Liveness Analysis: Calculate validator uptime and reliability

3. Performance Tracking: Identify missed attestations
Reward Prediction: Estimate rewards based on attestation frequency

Notes
1. Sorted by epoch ID (descending) - most recent first
2. Missing epochs in the response indicate missed attestations
3. Use with validator liveness metrics for comprehensive reliability analysis
4. Each attestation represents successful participation in an epoch

---


Operation Types

Validator Operations:
1. validator_stake - Validator stakes own tokens
2. validator_withdrawal_initiated - Validator initiates withdrawal
3. validator_withdrawal_completed - Validator completes withdrawal
4. validator_claim_rewards - Validator claims rewards

Delegator Operations:
1. delegator_stake - Delegator stakes to this validator
2. delegator_withdrawal_initiated - Delegator initiates withdrawal
3. delegator_withdrawal_cancelled - Delegator cancels withdrawal
4. delegator_withdrawal_completed - Delegator withdraws
5. delegator_move_stake - Delegator moves stake
6. delegator_claim_rewards - Delegator claims rewards