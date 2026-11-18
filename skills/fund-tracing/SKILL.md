---
name: fund-tracing
description: Trace where funds came from and went with Bitquery transfer data on 19 chains. Use when the user asks about money movement ("where did this wallet send its funds", "trace this to an exchange", "who funded this wallet", "did these two wallets transact", "show transfers in this transaction", "which Bitcoin addresses share an owner").
compatibility: Needs the Bitquery MCP server (https://mcp.bitquery.io/mcp) connected in Hermes as `bitquery`, signed in to a Bitquery account. Tracing needs a Bitquery plan that includes it.
---

# Fund tracing

Everything comes from the Bitquery MCP tools. If they are not available, or a Bitquery tool says to reconnect, tell the user how to connect Bitquery and stop: the first time, `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth --connect-timeout 300` for a browser sign-in, or, where no browser can open (a server, SSH, scheduled runs), `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth header --connect-timeout 300` and paste a Bitquery access token from account.bitquery.io when Hermes asks; after that, `hermes mcp login bitquery` for a browser sign-in, or, if the token expired (`HTTP 401`), a new token in place of `MCP_BITQUERY_API_KEY` in the profile's `.env`. Either way, a new session then loads the tools. Never ask the user for an API key or token.

## 1. Find the chain's tools

Tracing tools are named `<chain>_<capability>`. Prefixes: `eth_`, `bsc_`, `base_`, `arbitrum_`, `optimism_`, `matic_` (Polygon), `tron_`, `robinhood_`, `arc_`, `solana_`, `xrp_`, `xlm_` (Stellar), `btc_`, `ltc_`, `doge_`, `bch_`, `dash_`, `zec_`, `ada_` (Cardano). Not every chain has every capability. If unsure, call `chain_capabilities` with `chain`. For a full investigation, `prompt_playbook` returns step-by-step instructions.

## 2. Account chains (Ethereum and EVM chains, Tron, Solana, XRP, Stellar)

| The user asks about | Call |
|---|---|
| Where did the funds go, by counterparty | `<chain>_trace_next_hop` with `address` |
| Follow the largest flow of one token automatically | `<chain>_trace_dominant_path` with `address` and `currency` |
| Outgoing or incoming transfers one by one | `<chain>_transfers_out`, `<chain>_transfers_in` with `address` |
| Top counterparties in both directions | `<chain>_address_flow_summary` with `address` |
| Counts and totals for an address | `<chain>_address_profile` with `address` |
| Graph edges for a flow diagram | `<chain>_flow_edges` with `address` |
| All transfers inside a transaction | `<chain>_tx_transfers` with `tx_hash` |
| Who first funded these wallets (EVM chains and Tron) | `<chain>_first_funder` with `addresses` |
| Did two sets of addresses transact (EVM chains and Tron) | `<chain>_flows_between` with `addresses_a` |

## 3. Bitcoin-like chains (Bitcoin, Litecoin, Dogecoin, Bitcoin Cash, Dash, Zcash, Cardano)

| The user asks about | Call |
|---|---|
| Coins an address received | `<chain>_address_received` with `address` |
| Transactions where an address spent coins | `<chain>_sent_from_address` with `address` |
| All inputs and outputs of a transaction | `<chain>_tx_flow` with `tx_hash` |
| Addresses that likely share an owner | `<chain>_related_addresses` with `address` |
| A ledger for a set of addresses | `<chain>_wallet_ledger` with `addresses` |
| Top counterparties, profile, graph edges | `<chain>_address_flow_summary`, `<chain>_address_profile`, `<chain>_flow_edges` with `address` |

## 4. Trace one hop at a time

Follow the largest outgoing flows one hop at a time. Label each new address with `address_labels` (or many with `labels_for_addresses`) before going further. Stop at the first address labelled as an exchange, bridge or mixer, and say that the trail cannot be followed past it with on-chain data alone. Use `price_at` to value a transfer in USD at the time it happened.

## Rules

- A label or a transfer is evidence, not a verdict. Write "labelled as" and "sent funds to", never that a person or company committed a crime.
- A link between two addresses does not prove they share an owner. Say what the link is and how strong it is. The shared-owner result on Bitcoin-like chains is a heuristic.
- Only successful transactions are in the data.
- If a tool returns an access error, the user's plan does not include that tool. Say so and stop; do not retry.
- Give times in UTC with the date and year, and write addresses in full.
