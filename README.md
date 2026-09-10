# My-trading-skills

The index of the skills I use for trading in Claude Code, packaged as one plugin
marketplace. Each skill lives in its upstream repository; this repo only lists
them. Same construction as [`academic-skills`](https://github.com/yushiran/academic-skills).

## Install

```sh
claude plugin marketplace add yushiran/My-trading-skills
claude plugin install binance@yushiran-trading
```

Or `/plugin` inside Claude Code and pick from `yushiran-trading`. Skills are
namespaced: `yushiran-trading:binance`.

## Plugins

### Broker and exchange APIs

| Plugin | Repository | Skills taken | Tracks |
| --- | --- | --- | --- |
| `ibkr` | this repo, `plugins/ibkr` — wraps [IBKR's official hosted MCP](https://www.interactivebrokers.com/en/trading/ai-integrations.php) | `ibkr` **+ the MCP server itself** | local |
| `trading212-api` | [trading212-labs/agent-skills](https://github.com/trading212-labs/agent-skills) | `trading212-api` | `master` |
| `binance` | [binance/binance-skills-hub](https://github.com/binance/binance-skills-hub) | `binance`, `fiat`, `p2p`, `payment`, `onchain-pay`, `square-post`, `academy-skill` | `main` |
| `binance-web3` | [binance/binance-skills-hub](https://github.com/binance/binance-skills-hub) | the 12 `binance-web3` skills: agentic wallet, wallet and whale tracking, token info and audits, market rank, meme rush, trading signals, leaderboard | `main` |
| `futu-openapi` | [FutunnOpen/futu-agent-hub](https://github.com/FutunnOpen/futu-agent-hub) | `futuapi`, `install-futu-opend` | `main` |

### Research and strategy

| Plugin | Repository | Skills taken | Tracks |
| --- | --- | --- | --- |
| `ai-berkshire` | [xbtlin/ai-berkshire](https://github.com/xbtlin/ai-berkshire) | 21 value-investing workflows from `codex-skills/` | `main` |
| `trading-skills` | [tradermonty/claude-trading-skills](https://github.com/tradermonty/claude-trading-skills) | all 74 — screeners, technical and breadth analysis, regime detection, position sizing, pre-trade gates, drawdown circuit breaker | `main` |
| `quant-defi-skills` | [agiprolabs/claude-trading-skills](https://github.com/agiprolabs/claude-trading-skills) | all 68 — market microstructure, regime detection, Kelly sizing, risk management, volatility modelling, on-chain and whale analysis | `main` |

Everything tracks its upstream default branch, so a push upstream is picked up on
the next update. Nothing is vendored — no skill content is redistributed here.

Broker APIs change, and a stale skill against a changed API is worse than an
unreviewed one, which is why these are not pinned by commit.

## `ibkr` installs an MCP server, not just a skill

IBKR published an official hosted MCP endpoint on 2026-07-28, so there is nothing to
run locally — no gateway, unlike the Futu OpenD path. `plugins/ibkr` carries the
`.mcp.json` that registers it plus a SKILL.md with the working rules, so:

```sh
claude plugin install ibkr@yushiran-trading
```

registers `https://api.ibkr.com/v1/api/mcp-public` as well. Use the `-public` endpoint, not
`/mcp`: the latter is reserved for platforms' certified connectors and rejects a generic MCP
client with `400 Unsupported client` *after* the OAuth handshake succeeds. The first call opens a browser
for OAuth sign-in. **Orders never execute automatically** — the server drafts
instructions and execution stays in IBKR's own interface.

Schwab has no equivalent: confirmed 2026-09-10 that the Trader API is not offered to
international (non-US) clients, so there is nothing to wire up. The community
`schwab-mcp` servers are therefore moot for this account.

## Caveats worth knowing

- **`ai-berkshire` ships tools the plugin does not carry.** Several of its
  workflows shell out to `tools/financial_rigor.py` in the upstream repository;
  installing the skills alone leaves those calls unresolved. Clone the repo
  separately if you want the exact three-scenario and valuation arithmetic rather
  than the model doing it by hand.
- **`trading-skills` and `quant-defi-skills` are large third-party suites** (74
  and 68 skills). They are indexed whole; prune the `skills` array here if the
  breadth gets noisy.

## Not in the marketplace: Futu's analysis skills

Futu publishes six more skills only as a zip on `futunn.com`, not on GitHub:

- `futu-news-search`, `futu-stock-digest`, `futu-comment-sentiment`
- `futu-capital-anomaly`, `futu-derivatives-anomaly`, `futu-technical-anomaly`

There is no git source to track, and mirroring content Futu chose not to publish
on GitHub is not something this repo does. Install them from the vendor instead:

```sh
# official installer manifest; fetches https://www.futunn.com/skills/futu.zip
curl -L https://www.futunn.com/skills/futu-install.md
```

See the [Futu Skills Hub](https://www.futunn.com/en/skillhub). If Futu later adds
them to `futu-agent-hub`, move them into `marketplace.json` and delete this
section.

## Add a skill

1. Make sure the skill has a git source. Plugin-shaped means a `SKILL.md` at the
   repository root, or a `skills/` directory with one subdirectory per skill.
2. Add an entry to `.claude-plugin/marketplace.json` with `"ref"` set to the
   upstream default branch. Check what that branch is actually called — Trading
   212 uses `master`, everyone else here uses `main`.
3. Use the `skills` array to take only part of a hub; omit it to take everything
   the repository exposes.
4. Run `claude plugin validate .` here, and confirm each path really exists at
   that ref, before pushing.
5. Add a row to the table above.
