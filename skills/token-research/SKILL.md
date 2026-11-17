---
name: token-research
description: Research a crypto token or asset with Bitquery on-chain data, covering price, supply, price history, where it trades, launches, early buyers and who is accumulating. Use when the user asks about a token ("price of WIF", "chart for this token", "what's trending on Solana", "new tokens today", "which chains is USDC on", "who bought this first", "what was ETH worth on that date").
compatibility: Needs the Bitquery MCP server (https://mcp.bitquery.io/mcp) connected in Hermes as `bitquery`, signed in to a Bitquery account.
---

# Token research

Everything comes from the Bitquery MCP tools. If they are not available, or a Bitquery tool says to reconnect, tell the user how to connect Bitquery and stop: the first time, `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth --connect-timeout 300`; after that, `hermes mcp login bitquery`. Either way, a new session then loads the tools. Never ask the user for an API key or token.

## 1. Pin down the token first

A name or ticker is not a token. Many tokens share a symbol, and copies of popular ones are common.

| The user gave | Call |
|---|---|
| A contract or mint address | `find_token_by_address` |
| A token name or ticker | `find_tokens` with `query` |
| A well-known asset across all chains (BTC, ETH, USDC) | `find_currencies` with `query`, which returns a `currency_id` |
| Which chains a token is on | `token_chains` with `query` |

If more than one token matches, show the top matches with chain, address and volume and ask which one, unless one clearly dominates by volume. Carry the address and chain into every later call.

## 2. Choose the tool by the question

| The user asks about | Call |
|---|---|
| Current price, market cap, FDV of a token | `token_price` with `address` |
| Supply of a token | `token_supply` with `address` |
| Price history or a chart of a token | `token_ohlcv` with `address` |
| Bars built from individual trades (buys, sells, wallets per bar) | `token_trade_bars` |
| Price, supply or candles of an asset across all chains | `currency_price`, `currency_supply`, `currency_ohlcv` with `currency_id` |
| What a token or coin was worth at a past moment | `price_at` with `chain` and `items` |
| Which DEXs and pools a token trades on | `token_dex_venues` with `address` |
| What is trending, top gainers or losers | `trending_tokens` |
| Newly launched tokens | `new_tokens` |
| How one launch went | `token_launch_stats` |
| How many launches in a window survived or grew | `launch_cohort` with `blockchain` |
| First buyers or snipers | `early_buyers` with `address` and `blockchain` |
| Who is accumulating the token | `accumulating_traders_by_token` with `address` and `blockchain` |
| Is the data current, how far back does it go | `data_coverage` |

For rankings of traders and for individual trades, use the `dex-trades-and-traders` skill.

## 3. Write the answer

Lead with the number the user asked for, then the context. Use a short table for rankings and candles. Show the token's chain and full contract address once, so the user can check it is the token they meant.

## Rules

- Report volume as given. If a token has large volume from very few wallets, say so: it can be wash trading or a counterfeit.
- Do not give buy or sell advice. Report what the data shows.
- If a tool returns an access error, the user's plan does not include that tool. Say so and stop; do not retry.
- Give times in UTC with the date and year, and write addresses in full.
