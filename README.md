# Bitquery

Bitquery brings on-chain data into Hermes Agent. Ask about token prices, DEX trades, traders, wallets and prediction markets in plain English, and Hermes answers from Bitquery's hosted MCP server.

## What you can ask

- "What are the trending Solana tokens in the last 24 hours?"
- "Show the price history of this token for the past week."
- "Who were the first buyers of this token, and who made the most?"
- "What labels does this address have? Is it an exchange deposit wallet?"
- "Where did this wallet send its funds?"
- "What are the odds on the Fed rate market on Polymarket?"
- "How has BTC funding moved on Hyperliquid today?"

## What's included

**Skills**

- **Token research**: find a token or asset, then its price, supply, candles, venues, launch, early buyers and accumulators.
- **DEX trades and traders**: pair prices and candles, individual swaps, trader leaderboards and what a wallet traded.
- **Wallet check**: labels, a risk report and the counterparties of an address.
- **Fund tracing**: where funds came from and went, on 19 chains.
- **Contract and on-chain activity**: contract creators, calls, events, token holders, transaction history and balances.
- **Hyperliquid**: markets, funding, open interest, liquidations, order book, accounts and vaults.
- **Prediction markets**: Polymarket and Hyperliquid markets, odds, holders and traders.
- **Custom SQL**: read-only SQL for questions no other tool answers.

**The Bitquery MCP server** (`https://mcp.bitquery.io/mcp`) serves DEX trades, prices, candles, trader PnL, transfer tracing, address labels, Polymarket and Hyperliquid data. Chains include Ethereum, Solana, BNB Chain, Base, Arbitrum, Optimism, Polygon, Tron and Bitcoin. Full list: [docs.bitquery.io/docs/mcp/mcp-server](https://docs.bitquery.io/docs/mcp/mcp-server/).

## Get started

1. Install the plugin:

   ```
   hermes plugins install bitquery --enable
   ```

2. Connect your Bitquery account:

   ```
   hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth --connect-timeout 300
   ```

   Hermes opens Bitquery's sign-in in your browser. A new account starts with a free 7-day trial. The plugin's own `mcp.json` does not sign you in, so this step is needed.

3. Start a new session and ask one of the questions above.

If a Bitquery tool later asks you to reconnect, run `hermes mcp login bitquery`. Scheduled and unattended runs can't open a browser, so they report it instead.

## Data and privacy

The plugin is read-only. Every Bitquery tool reads public blockchain data; none signs or sends a transaction, places an order or changes anything in your Bitquery account.

Every tool call goes to Bitquery's server at `mcp.bitquery.io`, signed in as you. The addresses, tokens and questions in a call are sent to Bitquery, and each call uses credits from your Bitquery plan. What Hermes reads back joins your conversation, so it reaches the model provider your Hermes profile uses.

Hermes keeps the Bitquery sign-in in your profile and refreshes it. The plugin has no code of its own, only these eight skills and the server address, and it stores nothing. The tool list comes from Bitquery's server when Hermes connects, so it can change without a new plugin version.

To disconnect, run `hermes mcp remove bitquery` and `hermes plugins disable bitquery`.

[Privacy policy](https://bitquery.io/privacy-policy) · [Terms](https://bitquery.io/terms-of-service) · Help: [support@bitquery.io](mailto:support@bitquery.io)
