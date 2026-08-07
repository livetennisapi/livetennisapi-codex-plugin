# Live Tennis API — Codex plugin

[![ci](https://github.com/livetennisapi/livetennisapi-codex-plugin/actions/workflows/ci.yml/badge.svg)](https://github.com/livetennisapi/livetennisapi-codex-plugin/actions/workflows/ci.yml)
[![license: MIT](https://img.shields.io/badge/license-MIT-blue.svg)](LICENSE)

Live tennis scores, fixtures, players, results, odds and model analysis for
[OpenAI Codex](https://developers.openai.com/codex/), via the hosted
[Live Tennis API](https://livetennisapi.com/) MCP server — ATP, WTA,
Challenger, ITF and juniors.

Twelve read-only tools, no local install — the server is hosted at
`https://mcp.livetennisapi.com/mcp` and Codex talks to it over streamable
HTTP. (The [`livetennisapi-mcp`](https://github.com/livetennisapi/livetennisapi-mcp)
npm package also runs locally over stdio and adds further tools — results
archive, rankings, in-play statistics, charting; see its README.)

## Requirements

- Codex CLI **v0.117.0 or newer** (plugins landed in 0.117.0). Verified against
  `codex-cli 0.130.0`.
- A Live Tennis API key — [get a free one](https://livetennisapi.com/subscribe/free).

## Get your key into the environment

Both install routes read the key from the `LIVETENNISAPI_KEY` environment
variable. The key is never written into a config file.

```bash
export LIVETENNISAPI_KEY="twjp_your_key_here"   # add to ~/.bashrc or ~/.zshrc
```

`tools/list` works without a key; individual tool calls need one.

## Install — route 1: marketplace

Add this repo as a plugin marketplace:

```bash
codex plugin marketplace add livetennisapi/livetennisapi-codex-plugin
```

Then open Codex and install **Live Tennis API** from the plugin picker.

> On Codex 0.130.0 the `codex plugin` CLI only manages *marketplaces*
> (`add` / `upgrade` / `remove`) — there is no `codex plugin install`
> subcommand. Adding the marketplace registers it; installing the plugin from
> it is done inside Codex. If you would rather not use the picker, use route 2,
> which is a single command and does not depend on the plugin system at all.

To remove the marketplace again:

```bash
codex plugin marketplace remove livetennisapi
```

## Install — route 2: plain MCP server (one command)

This skips the plugin system and registers the endpoint directly. It is the
fastest path and works on any Codex with remote MCP support:

```bash
codex mcp add livetennisapi \
  --url https://mcp.livetennisapi.com/mcp \
  --bearer-token-env-var LIVETENNISAPI_KEY
```

That writes the following into `~/.codex/config.toml`:

```toml
[mcp_servers.livetennisapi]
url = "https://mcp.livetennisapi.com/mcp"
bearer_token_env_var = "LIVETENNISAPI_KEY"
```

You can also add that block by hand. Use `bearer_token_env_var`, not
`bearer_token` — the latter would put your key in a plaintext config file.

Verify it registered:

```bash
codex mcp list
# Name           Url                                Bearer Token Env Var  Status   Auth
# livetennisapi  https://mcp.livetennisapi.com/mcp  LIVETENNISAPI_KEY     enabled  Bearer token
```

Remove with `codex mcp remove livetennisapi`.

## Tools

| Tool | What it returns | Plan |
|---|---|:--:|
| `get_live_matches` | Matches in progress, with live scores | FREE |
| `get_upcoming_matches` | Matches scheduled to start soon | FREE |
| `get_match` | Full detail for one match: players, score, surface, round, status | FREE |
| `get_match_score` | Current score for one match — lowest-latency read | FREE |
| `get_fixtures` | Forward schedule, earliest first | FREE |
| `search_players` | Search players by name | FREE |
| `get_player` | Player profile: ranking, country, handedness, date of birth | FREE |
| `check_api_status` | Reachability check and which plan your key is on | FREE |
| `get_recent_results` | Recently completed matches with final scores | BASIC |
| `get_match_events` | Event timeline: breaks, games won, sets won, momentum | PRO |
| `get_match_odds` | Match-winner market prices and implied probability | PRO |
| `get_match_analysis` | Model win probability and reasoning | ULTRA |

All tools are read-only. Tools above your plan are not errors — they return a
plain-English note naming the plan needed. `get_recent_results` is also
unlocked by any History plan, which works on top of a free key.

## Plans and quotas

`check_api_status` reports the plan attached to your key.

| Plan | Adds | Per minute | Per day | Price |
|---|---|---:|---:|---|
| FREE | Live and upcoming matches, scores, players, fixtures | 30 | 100 | $0 |
| BASIC | + recent results (also via any History plan) | 60 | 1,000 | $9.99/mo |
| PRO | + match events and market prices | 300 | 10,000 | $29.99/mo |
| ULTRA | + model analysis and win probability | 600 | 500,000 | $99.99/mo |

On a free key (100 requests/day), poll no faster than every 15 minutes; for an
always-on dashboard, BASIC is recommended.

## Try it

```
Which tennis matches are live right now?
Show me today's upcoming ATP fixtures.
What are the odds on the Alcaraz match?
```

## Links

- [Live Tennis API](https://livetennisapi.com/)
- [Docs](https://docs.livetennisapi.com)
- [Free key](https://livetennisapi.com/subscribe/free)
- [Discord](https://discord.gg/f8WUZHgDm6)
- [GitHub org](https://github.com/livetennisapi)
- [MCP server source](https://github.com/livetennisapi/livetennisapi-mcp)

## License

MIT — see [LICENSE](./LICENSE).

## Affiliate program

Know developers who need tennis data? The [affiliate program](https://affiliates.livetennisapi.com/program) pays 51% recurring commission for the life of every referred subscription — 30-day cookie, and the people you refer get 10% off.
