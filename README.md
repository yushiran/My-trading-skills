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

| Plugin | Repository | Skills taken | Tracks | Notes |
| --- | --- | --- | --- | --- |
| `trading212-api` | [trading212-labs/agent-skills](https://github.com/trading212-labs/agent-skills) | `trading212-api` | pinned sha | official; ISA + Invest accounts, orders, positions, history |
| `binance` | [binance/binance-skills-hub](https://github.com/binance/binance-skills-hub) | `skills/binance/binance` | pinned sha | official; needs `binance-cli`. The hub also ships `fiat`, `p2p`, `payment`, `onchain-pay`, `square-post`, `academy-skill` and a `binance-web3` set — deliberately not taken |
| `futu-openapi` | [FutunnOpen/futu-agent-hub](https://github.com/FutunnOpen/futu-agent-hub) | `futuapi`, `install-futu-opend` | pinned sha | official; quotes, financials, trading via OpenD |

Everything here is third-party and pinned by commit, so an upstream change never
lands unreviewed. Nothing in this repo is vendored — no skill content is
redistributed.

## Not in the marketplace: Futu's analysis skills

Futu publishes six more skills only as a zip on `futunn.com`, not on GitHub:

- `futu-news-search`, `futu-stock-digest`, `futu-comment-sentiment`
- `futu-capital-anomaly`, `futu-derivatives-anomaly`, `futu-technical-anomaly`

There is no git source to pin, and mirroring content Futu chose not to publish on
GitHub is not something this repo does. Install them from the vendor instead:

```sh
# official installer manifest; fetches https://www.futunn.com/skills/futu.zip
curl -L https://www.futunn.com/skills/futu-install.md
```

See the [Futu Skills Hub](https://www.futunn.com/en/skillhub). If Futu later adds
them to `futu-agent-hub`, move them into `marketplace.json` and delete this
section.

## Update

Every plugin is pinned by commit. A weekly action moves each pin to upstream HEAD
and opens an issue with the compare link, so the change gets read before it is
used; `/plugin marketplace update yushiran-trading` then fetches the new manifest.
Run it by hand from the Actions tab any time.

## Add a skill

1. Make sure the skill has a git source. Plugin-shaped means a `SKILL.md` at the
   repository root, or a directory per skill that `skills` can point at.
2. Add an entry to `.claude-plugin/marketplace.json`. Use `"ref": "main"` for a
   repository I own and `"sha"` for anyone else's — third-party code gets pinned.
3. Take only the skills actually used, via the `skills` array, rather than
   installing a whole hub.
4. Run `claude plugin validate .` here before pushing.
5. Add a row to the table above.
