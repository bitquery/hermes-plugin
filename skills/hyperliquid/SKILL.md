---
name: hyperliquid
description: Read Hyperliquid perpetuals, spot and vault data with Bitquery. Use when the user asks about Hyperliquid ("top Hyperliquid markets today", "BTC funding rate", "open interest", "who got liquidated", "largest positions", "what does this wallet hold on Hyperliquid", "who holds HYPE", "who is in the HLP vault").
compatibility: Needs the Bitquery MCP server (https://mcp.bitquery.io/mcp) connected in Hermes as `bitquery`, signed in to a Bitquery account.
---

# Hyperliquid

Everything comes from the Bitquery MCP tools. If they are not available, or a Bitquery tool says to reconnect, tell the user how to connect Bitquery and stop: the first time, `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth`; after that, `hermes mcp login bitquery`. Either way, a new session then loads the tools. Never ask the user for an API key or token.

## 1. Markets

| The user asks about | Call |
|---|---|
| Which markets are most traded | `hyperliquid_markets` |
| Current price of a market | `hyperliquid_price` with `market` |
| Candles | `hyperliquid_candles` with `market` |
| Individual trades | `hyperliquid_recent_trades` with `market` |
| Open interest, long and short skew, current funding | `hyperliquid_open_interest` |
| Funding and open interest history | `hyperliquid_funding_rates` with `market` |
| How many accounts are long or short | `hyperliquid_market_positioning` with `market` |
| Largest open positions | `hyperliquid_largest_positions` |
| Liquidations | `hyperliquid_liquidations` |

## 2. Order book and order flow

| The user asks about | Call |
|---|---|
| Largest resting orders and who owns them | `hyperliquid_book_walls` with `market` |
| Who provides the liquidity | `hyperliquid_book_makers` with `market` |
| Pending stop and take-profit orders by price | `hyperliquid_stop_map` with `market` |
| TWAP orders being worked | `hyperliquid_twaps` |
| Orders placed, filled, cancelled, rejected | `hyperliquid_order_flow` |
| Actions the exchange rejected | `hyperliquid_action_rejections` |

## 3. Accounts

| The user asks about | Call |
|---|---|
| Leaderboard by PnL, volume or fills | `hyperliquid_top_traders` |
| An account's open positions | `hyperliquid_trader_positions` with `trader` |
| An account's whole book: margin, positions, spot | `hyperliquid_trader_portfolio` with `trader` |
| An account's PnL, funding and fees over a period | `hyperliquid_trader_summary` with `trader` |
| An account's fills | `hyperliquid_trader_fills` with `trader` |
| Accounts run by the same operator | `hyperliquid_linked_accounts` with `trader` |

## 4. Spot tokens, vaults and builders

| The user asks about | Call |
|---|---|
| Holders of a spot token | `hyperliquid_token_holders` with `token` |
| Vaults ranked by capital | `hyperliquid_vaults` |
| Depositors of one vault | `hyperliquid_vault_depositors` with `vault` |
| Revenue of front-ends and bots that route orders | `hyperliquid_builders` |

For Hyperliquid prediction markets, use the `prediction-markets` skill.

## Rules

- Profit rankings leave out open positions. Read an account's positions before calling it a winner.
- Funding is quoted per period by the exchange. Say which period, or convert to a yearly rate and say so.
- These tools read data only. They cannot place, change or cancel an order.
- Do not give trading advice. Report what the data shows.
- If a tool returns an access error, the user's plan does not include that tool. Say so and stop; do not retry.
- Give times in UTC with the date and year, and write addresses in full.
