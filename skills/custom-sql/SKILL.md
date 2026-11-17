---
name: custom-sql
description: Answer a question with read-only SQL over Bitquery data when no purpose-built tool fits. Use only after checking the other Bitquery skills, for custom aggregations, joins or backtests ("write a query for", "aggregate across all pools", "custom cohort", "what tables are there", "which column holds the fee").
compatibility: Needs the Bitquery MCP server (https://mcp.bitquery.io/mcp) connected in Hermes as `bitquery`, signed in to a Bitquery account. The SQL tools need a paid Bitquery plan.
---

# Custom SQL

Everything comes from the Bitquery MCP tools. If they are not available, or a Bitquery tool says to reconnect, tell the user how to connect Bitquery and stop: the first time, `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth --connect-timeout 300`; after that, `hermes mcp login bitquery`. Either way, a new session then loads the tools. Never ask the user for an API key or token.

SQL is the last resort. A purpose-built tool is faster, cheaper in credits and harder to get wrong. Use SQL only when no tool in the other Bitquery skills answers the question.

## 1. Learn the schema first

| To find | Call |
|---|---|
| Which databases SQL can reach | `list_databases` |
| Tables of the trading database | `list_tables` |
| Columns of a trading table | `describe_table` with `table` |
| Find a trading column by name | `search_columns` with `query` |
| Tables of a per-chain transfer database | `chain_list_tables` with `database` |
| Columns of a per-chain table | `chain_describe_table` with `database` and `table` |
| Find a per-chain column by name | `chain_search_columns` with `database` and `query` |

Never guess a table or column name. Look it up.

## 2. Run the query

| The data | Call |
|---|---|
| DEX trades, prices, pairs, traders | `execute_sql` with `sql` |
| Transfers on one chain | `<chain>_transfers_raw_sql` with `sql`, for example `eth_transfers_raw_sql`, `tron_transfers_raw_sql`, `btc_transfers_raw_sql` |
| Hyperliquid | `hyperliquid_raw_sql` with `sql` |

Always filter by a time range and add a `LIMIT`. Start with a small window and widen it only if needed. Call `data_coverage` to see how far back the trading data goes.

## 3. Write the answer

Show the result first, then the query in a code block, so the user can check and reuse it.

## Rules

- All SQL here is read-only. Do not attempt to write, alter or drop anything.
- If a query fails, read the error, fix the query once, and say what changed. Do not retry the same query.
- Large scans use more credits. Tell the user before running a query over a long history.
- If a tool returns an access error, the user's plan does not include that tool. Say so and stop; do not retry.
- Give times in UTC with the date and year, and write addresses in full.
