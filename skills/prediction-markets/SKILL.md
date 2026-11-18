---
name: prediction-markets
description: Read prediction markets on Polymarket and Hyperliquid with Bitquery on-chain data. Use when the user asks about prediction markets or betting odds ("odds on the Fed rate market", "top Polymarket markets today", "find markets about the election", "who holds Yes", "biggest Polymarket traders", "what did this wallet bet on", "which markets were disputed").
compatibility: Needs the Bitquery MCP server (https://mcp.bitquery.io/mcp) connected in Hermes as `bitquery`, signed in to a Bitquery account.
---

# Prediction markets

Everything comes from the Bitquery MCP tools. If they are not available, or a Bitquery tool says to reconnect, tell the user how to connect Bitquery and stop: the first time, `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth --connect-timeout 300` for a browser sign-in, or, where no browser can open (a server, SSH, scheduled runs), `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth header --connect-timeout 300` and paste a Bitquery access token from account.bitquery.io when Hermes asks; after that, `hermes mcp login bitquery` for a browser sign-in, or, if the token expired (`HTTP 401`), a new token in place of `MCP_BITQUERY_API_KEY` in the profile's `.env`. Either way, a new session then loads the tools. Never ask the user for an API key or token.

## 1. Polymarket markets

| The user asks about | Call |
|---|---|
| The most traded markets | `polymarket_top_markets` |
| Markets about a topic | `polymarket_find_markets` with `query` |
| Full detail and outcome IDs of markets | `polymarket_market_info` |
| The odds on one market | `polymarket_market_odds` |
| How the odds moved over time | `polymarket_market_price_history` |
| Individual fills in a market | `polymarket_market_trades` |
| Who holds an outcome right now | `polymarket_outcome_holders` with `question_id` |
| Markets created or resolved recently | `polymarket_market_events` |
| Markets whose answer was challenged | `polymarket_market_disputes` |
| Redemptions, splits and merges | `polymarket_settlements` |

To get odds for a market the user named in words, call `polymarket_find_markets` first, pick the matching market, then pass its ID to `polymarket_market_odds`. If several markets match, list them and ask which one.

## 2. Polymarket traders

| The user asks about | Call |
|---|---|
| Wallets ranked by traded volume | `polymarket_top_traders` |
| A wallet's lifetime card | `polymarket_trader_summary` with `trader_address` |
| What a wallet holds now | `polymarket_trader_positions` with `trader_address` |
| Everything a wallet did, per market | `polymarket_trader_activity` with `trader_address` |

## 3. Hyperliquid prediction markets

| The user asks about | Call |
|---|---|
| The most traded markets | `hyperliquid_prediction_markets` |
| Answers, sides and odds of one market | `hyperliquid_prediction_market` with `market` |
| Who holds one side | `hyperliquid_prediction_holders` with `market` |

## 4. Write the answer

For odds, give each outcome with its implied probability as a percentage, the traded volume and the time of the last trade. For a ranking, use a short table.

## Rules

- The odds are the last traded price, not a forecast. If the last trade is old or the volume is small, say so.
- The Polymarket trader ranking is by volume and carries no profit figure. A wallet with most of its orders resting on the book is likely a market maker.
- These tools read data only. They cannot place, change or cancel a bet.
- Do not give betting advice. Report what the data shows.
- If a tool returns an access error, the user's plan does not include that tool. Say so and stop; do not retry.
- Give times in UTC with the date and year, and write addresses in full.
