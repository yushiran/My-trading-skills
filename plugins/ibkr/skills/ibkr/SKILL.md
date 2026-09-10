---
name: ibkr
description: Read an Interactive Brokers account through IBKR's official hosted MCP server — positions, cash and multi-currency balances, margin availability, realised and unrealised P&L, historical transactions, option chains, risk exposures and linked-account structure. Use when the user mentions IBKR, Interactive Brokers, TWS, their IB account, or asks to check positions, balances, margin, P&L or transactions there, or to compare IBKR holdings against another broker. Installing this plugin registers the MCP server; the first call opens a browser for OAuth sign-in.
---

# Interactive Brokers

This plugin ships IBKR's **official hosted MCP server** (`https://api.ibkr.com/v1/api/mcp`),
announced 2026-07-28. Installing the plugin registers it; nothing runs locally — unlike
the Futu path there is no gateway to start.

## First run

The first call opens a browser to sign in with the IBKR account and authorise access.
Credentials are reused afterwards. If the tools are missing, check `claude mcp list`.

## What it exposes

Positions · cash and multi-currency balances · margin availability · realised and
unrealised P&L · historical transactions · option chains · risk exposures · linked
account structure.

## The safety property that matters

**Orders never execute automatically.** The server drafts trade instructions; execution
stays with the user in IBKR's own interface. Treat any order this skill produces as a
draft to be reviewed and placed by hand — do not describe a drafted instruction as a
placed trade.

## Working rules

- **Read-only by default.** Use it to answer questions about the account. Do not
  volunteer trade drafts unless asked for one.
- **State the currency.** The account is multi-currency; a bare number is ambiguous.
  Say which currency a balance or P&L figure is in.
- **Realised vs unrealised.** Keep them apart when reporting performance — a portfolio
  can show unrealised gains while realised P&L is negative.
- **Cross-broker questions.** When comparing against Trading 212, Futu or Binance
  holdings, convert to one currency and say the rate used. Look-through exposure via
  index funds counts: an index position already carries single-name weight, so adding
  the same name directly concentrates rather than diversifies.
- **Do not infer eligibility or tax treatment** from the API. Account type, domicile
  and withholding are not reliably represented; ask rather than guess.

## Troubleshooting

- Tools absent → `claude mcp list`, then re-authenticate.
- Auth loops → remove and re-add: `claude mcp remove ibkr` then reinstall this plugin.
- The endpoint is hosted by IBKR; an outage there is not a local problem.
