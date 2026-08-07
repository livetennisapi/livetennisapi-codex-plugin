# Changelog

All notable changes to this plugin are documented here. The format follows
[Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [1.1.0] - 2026-08-07

### Added
- Plan column in the tool table and a combined plans + quotas table reflecting
  the quota grid of 2026-08-06: FREE 100/day, BASIC 1,000/day, PRO 10,000/day,
  ULTRA 500,000/day — with free-key polling guidance.
- Pointer to the `livetennisapi-mcp` npm package (local stdio) for the wider
  tool surface: results archive, rankings, in-play statistics, charting.
- CI and license badges; docs/Discord/org links.
- `scripts/truthcheck.sh` ground-truth guard, wired into CI.

### Changed
- Tour phrasing: ATP, WTA, Challenger, ITF and juniors (README and plugin
  manifest); keywords extended to match.
- ULTRA row no longer claims a "live feed" — the hosted MCP endpoint exposes
  tools only.
- Key placeholder in the quickstart now uses the real `twjp_…` prefix.

### Notes
- The tool table intentionally stays at the 12 tools the hosted endpoint at
  `mcp.livetennisapi.com` actually serves.

## [1.0.0] - 2026-07-22

### Added
- Initial release: Codex plugin + self-hosted marketplace registering the
  hosted Live Tennis API MCP endpoint (streamable HTTP), 12 read-only tools.
