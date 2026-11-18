---
name: dex-trades-and-traders
description: Look at DEX trading with Bitquery on-chain data, covering pair prices and candles, individual swaps, trader leaderboards and what one wallet traded. Use when the user asks about a pair, a pool or a trader ("WETH/USDC price on Base", "latest trades in this pool", "top traders of this token", "most profitable wallets", "what did this wallet trade", "decode this swap transaction").
compatibility: Needs the Bitquery MCP server (https://mcp.bitquery.io/mcp) connected in Hermes as `bitquery`, signed in to a Bitquery account.
---

# DEX trades and traders

Everything comes from the Bitquery MCP tools. If they are not available, or a Bitquery tool says to reconnect, tell the user how to connect Bitquery and stop: the first time, `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth --connect-timeout 300` for a browser sign-in, or, where no browser can open (a server, SSH, scheduled runs), `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth header --connect-timeout 300` and paste a Bitquery access token from account.bitquery.io when Hermes asks; after that, `hermes mcp login bitquery` for a browser sign-in, or, if the token expired (`HTTP 401`), a new token in place of `MCP_BITQUERY_API_KEY` in the profile's `.env`. Either way, a new session then loads the tools. Never ask the user for an API key or token.

## 1. Choose the tool by the question

| The user asks about | Call |
|---|---|
| Latest price of a pair | `pair_price` with `base_address`, `quote_address`, `blockchain` |
| Candles for a pair | `pair_ohlcv` with `base_address`, `quote_address`, `blockchain` |
| Individual swaps of a token or one pool | `pool_recent_trades` |
| The swaps inside one transaction | `tx_trades` with `tx_hash` |
| Top traders of one token | `top_traders_by_token` with `address` and `blockchain` |
| Most profitable traders of one token | `profitable_traders_by_token` with `address` and `blockchain` |
| Top traders of one pair | `top_traders_by_pair` with `base_address`, `quote_address`, `blockchain` |
| Top traders on a whole network | `top_traders_by_network` with `blockchain` |
| A wallet's summary: volume, trade count, cash flow | `trader_profile` with `trader_address` |
| What a wallet holds or traded per token | `trader_positions` with `trader_address` |
| A wallet's trading over time | `trader_activity` with `trader_address` |
| A wallet's swaps one by one | `trader_trades` with `address` |

The wallet tools take up to 50 wallets in one call. To find a token's address first, use the `token-research` skill.

## 2. Write the answer

Use a table for leaderboards and trade lists. For a wallet, lead with the summary row, then positions.

## Rules

- Trader profit figures are estimates from trade cash flow. Say "estimated", and note that open positions are not counted.
- A wallet with very high volume and thousands of trades is often a bot or market maker. Say what the numbers suggest; do not name an owner unless a label says so.
- Do not give buy or sell advice. Report what the data shows.
- If a tool returns an access error, the user's plan does not include that tool. Say so and stop; do not retry.
- Give times in UTC with the date and year, and write addresses in full.
