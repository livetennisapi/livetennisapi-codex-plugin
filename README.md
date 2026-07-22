# Live Tennis API — Codex plugin

Live tennis scores, fixtures, players, results, odds and model analysis for
[OpenAI Codex](https://developers.openai.com/codex/), via the hosted
[Live Tennis API](https://livetennisapi.com/) MCP server.

Twelve read-only tools, ATP / WTA / Challenger coverage, no local install — the
server is hosted at `https://mcp.livetennisapi.com/mcp` and Codex talks to it
over streamable HTTP.

## Requirements

- Codex CLI **v0.117.0 or newer** (plugins landed in 0.117.0). Verified against
  `codex-cli 0.130.0`.
- A Live Tennis API key — [get a free one](https://livetennisapi.com/subscribe/free).

## Get your key into the environment

Both install routes read the key from the `LIVETENNISAPI_KEY` environment
variable. The key is never written into a config file.

```bash
export LIVETENNISAPI_KEY="your-key-here"   # add to ~/.bashrc or ~/.zshrc
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

| Tool | What it returns |
|---|---|
| `get_live_matches` | Matches in progress, with live scores |
| `get_upcoming_matches` | Matches scheduled to start soon |
| `get_match` | Full detail for one match: players, score, surface, round, status |
| `get_match_score` | Current score for one match — lowest-latency read |
| `get_match_events` | Event timeline: breaks, games won, sets won, momentum |
| `get_match_odds` | Match-winner market prices and implied probability |
| `get_match_analysis` | Model win probability and reasoning |
| `get_fixtures` | Forward schedule, earliest first |
| `get_recent_results` | Recently completed matches with final scores |
| `search_players` | Search players by name |
| `get_player` | Player profile: ranking, country, handedness, date of birth |
| `check_api_status` | Reachability check and which plan your key is on |

All tools are read-only.

## Plans

`check_api_status` reports the plan attached to your key.

| Plan | Adds |
|---|---|
| FREE | Live and upcoming matches, scores, players, fixtures |
| BASIC | + historical results |
| PRO | + match events and market prices |
| ULTRA | + model analysis, win probability, live feed |

## Try it

```
Which tennis matches are live right now?
Show me today's upcoming ATP fixtures.
What are the odds on the Alcaraz match?
```

## Links

- [Live Tennis API](https://livetennisapi.com/)
- [Free key](https://livetennisapi.com/subscribe/free)
- [MCP server source](https://github.com/livetennisapi/livetennisapi-mcp)

## License

MIT — see [LICENSE](./LICENSE).
