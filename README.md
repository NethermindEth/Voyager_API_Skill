# Voyager Developer Skill

An AI skill that provides production-grade guidance for integrating with the Voyager API on Starknet.

## What This Skill Covers

This repository packages a structured skill for AI agents that need to answer developer questions about Voyager's indexed REST API.

- Authentication and API key usage
- API status and health checks
- Blocks and transactions
- Contracts and classes
- Token transfers, holders, and balances
- Events and messages
- Network statistics
- NFT contracts, items, holders, balances, and transfers
- Staking overview, validators, delegators, wallet info, attestations, and historical stake data
- Pagination, rate limits, and common API errors
- Production integration best practices

## What This Skill Does Not Cover

- Smart contract development
- Transaction signing
- Direct Starknet RPC interaction
- General protocol design or consensus topics

## Repository Structure

- `skill/SKILL.md` - Main skill definition and activation guidance
- `skill/api-overview.md` - Base URL, endpoint index, request and response conventions
- `skill/authentication.md` - API key usage and security guidance
- `skill/status.md` - API status endpoint
- `skill/blocks.md` - Block endpoints and fields
- `skill/transactions.md` - Transaction and meta-transaction endpoints
- `skill/contracts.md` - Contract metadata, balances, and transfers
- `skill/classes.md` - Class lookups, verification, and source code
- `skill/tokens.md` - Token listing, holders, and transfer activity
- `skill/events.md` - Contract-emitted events
- `skill/messages.md` - Message endpoints
- `skill/statistics.md` - Network statistics and daily metrics
- `skill/nfts.md` - NFT contracts, balances, holders, items, transfers, and metadata refresh
- `skill/staking.md` - Staking overview, validators, delegators, wallet info, attestations, and historical stake data
- `skill/pagination.md` - Pagination patterns and iteration strategy
- `skill/rate-limits.md` - Rate-limit behavior and retry guidance
- `skill/common-errors.md` - Common API errors and troubleshooting
- `skill/best-practices.md` - Integration guidance and implementation recommendations
- `skill/production_architecture.md` - Production usage patterns
- `skill/examples.md` - Example prompts and responses
- `install.sh` - Shell installer for local skill installation

## API Base URL

```text
https://api.voyager.online/beta
```

All API requests require:

```text
x-api-key: YOUR_API_KEY
```


## Install With the Shell Script

Install for your user:

```bash
./install.sh
```

Install for the current project:

```bash
./install.sh --project
```

Install to a custom path:

```bash
./install.sh --path /custom/path/voyager-dev
```

## Usage

Once installed, the skill can be activated by asking about Voyager API integration topics such as:

- How do I retrieve a Starknet contract by address?
- How do I list token transfers for a contract?
- How do I paginate Voyager responses?
- How do I query NFT balances for an owner address?
- How do I get staking validators or delegator information?
- How do I look up wallet staking activity or stake history over time?
- What does a `401` or `429` response mean?

## Notes

- Voyager is an indexer, not an RPC node.
- The skill covers both page-based and cursor-based pagination patterns.
- Indexed data may be slightly behind the latest Starknet block.
- Some values are returned as raw integers or hex values and may require application-side formatting.