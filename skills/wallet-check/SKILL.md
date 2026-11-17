---
name: wallet-check
description: Find out what is known about a blockchain address with Bitquery, covering labels, a risk report and its counterparties. Use when the user asks who or what an address is ("who is this address", "is this an exchange wallet", "is this address risky", "screen this wallet", "list Binance deposit addresses", "who does this wallet transact with").
compatibility: Needs the Bitquery MCP server (https://mcp.bitquery.io/mcp) connected in Hermes as `bitquery`, signed in to a Bitquery account. Some tools need a Bitquery plan that includes them.
---

# Wallet check

Everything comes from the Bitquery MCP tools. If they are not available, or a Bitquery tool says to reconnect, tell the user how to connect Bitquery and stop: the first time, `hermes mcp add bitquery --url https://mcp.bitquery.io/mcp --auth oauth --connect-timeout 300`; after that, `hermes mcp login bitquery`. Either way, a new session then loads the tools. Never ask the user for an API key or token.

## 1. Choose the tool by the question

| The user asks about | Call |
|---|---|
| Labels of one address: exchange, mixer, gambling, scam, contract type | `address_labels` with `address` |
| Labels of many addresses at once | `labels_for_addresses` with `addresses` |
| A risk score with evidence | `address_risk_report` with `chain` and `address` (Tron, Bitcoin and Ethereum) |
| Who an address paid and was paid by, in USD | `counterparties` with `addresses` and `chain` |
| All addresses that carry a label | `find_label_values` with `query` to get the exact label, then `addresses_by_label` with `label_type` and `label_value` |

Start with `address_labels`. To follow where funds went, use the `fund-tracing` skill. For what a wallet traded, use `dex-trades-and-traders`.

## 2. Write the answer

Lead with the labels, then the risk score and the evidence behind it, then the main counterparties with their own labels.

## Rules

- A label or a transfer is evidence, not a verdict. Write "labelled as" and "sent funds to", never that a person or company committed a crime.
- No label does not mean safe. Say that the address is not in the label set.
- If a tool returns an access error, the user's plan does not include that tool. Say so and stop; do not retry.
- Give times in UTC with the date and year, and write addresses in full.
