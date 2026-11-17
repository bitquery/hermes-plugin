---
name: contract-activity
description: Look at smart contracts, token holders and raw on-chain activity with Bitquery. Use when the user asks about a contract, a token's holders or an address's transactions ("who deployed this contract", "who called this method", "find Transfer events on this contract", "top holders of this token", "transaction history of this address", "ETH balance of these wallets", "who created this Solana token").
compatibility: Needs the Bitquery MCP server (https://mcp.bitquery.io/mcp) connected in Hermes as `bitquery`, signed in to a Bitquery account. Some tools need a Bitquery plan that includes them.
---

# Contract and on-chain activity

Everything comes from the Bitquery MCP tools. If they are not available, or a Bitquery tool says to reconnect, tell the user how to connect Bitquery and stop: the first time, `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth`; after that, `hermes mcp login bitquery`. Either way, a new session then loads the tools. Never ask the user for an API key or token.

## 1. EVM chains and Tron

Tools are named `<chain>_<capability>` with the prefixes `eth_`, `bsc_`, `base_`, `arbitrum_`, `optimism_`, `matic_` (Polygon), `tron_`, `robinhood_`, `arc_`.

| The user asks about | Call |
|---|---|
| Who created a contract, or what a creator deployed | `<chain>_contract_creator` |
| Calls to a contract by method | `<chain>_find_calls` with `contract` |
| Event logs of a contract by event name | `<chain>_find_events` with `contract` |
| All event logs inside a transaction | `<chain>_tx_events` with `tx_hash` |
| Transaction history of an address | `<chain>_transactions` with `address` |
| Top holders of a token | `<chain>_token_holders` with `token` |
| Addresses that sent a token | `<chain>_token_senders` with `token` |
| Native coin balance of addresses | `<chain>_native_balances` with `addresses` |

## 2. Solana and Cardano

| The user asks about | Call |
|---|---|
| Who created a Solana token, or what a creator minted | `solana_token_creator` |
| A Solana address's transaction signatures | `solana_signatures` with `address` |
| Solana transactions by program instruction | `solana_find_instructions` |
| Mint and burn history of a Cardano token | `ada_mints` with `currency` |

If unsure which capabilities a chain has, call `chain_capabilities` with `chain`.

## 3. Write the answer

Use a table for holders, calls and events. Label large holders with `labels_for_addresses`, so exchanges and contracts are not mistaken for individuals.

## Rules

- Only successful transactions and calls are in the data. An empty result for a failed transaction is expected.
- A top holder is often an exchange, a bridge or a contract. Check labels before describing it as a person.
- If a tool returns an access error, the user's plan does not include that tool. Say so and stop; do not retry.
- Give times in UTC with the date and year, and write addresses in full.
