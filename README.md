<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/gillby-logo-light.svg">
    <img src="assets/gillby-logo.svg" alt="Gillby" width="260">
  </picture>
</p>

<h1 align="center">Gillby MCP</h1>

<p align="center">
  <b>Florida marine conditions for AI agents.</b> Tides, waves, wind, marine forecasts, hazards, red tide, rip currents, depth-aware boat routing and 80+ coastal spots, through one remote MCP server and a public HTTP API.
</p>

<p align="center">
  <a href="https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.OneWave-AI%2Fgillby"><img alt="MCP Registry" src="https://img.shields.io/badge/MCP%20Registry-io.github.OneWave--AI%2Fgillby-20394D"></a>
  <a href="#claude-code-plugin"><img alt="Claude Code plugin" src="https://img.shields.io/badge/Claude%20Code-plugin-D97A3C"></a>
  <a href="https://gillby.vercel.app/api/v1/openapi.json"><img alt="OpenAPI" src="https://img.shields.io/badge/OpenAPI-v1-62A6C3"></a>
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-20394D"></a>
</p>

Connect any MCP-compatible agent (Claude, ChatGPT, Cursor, Windsurf, Claude Code) to **Gillby**, the AI guide to Florida water. Public data needs **no API key, no login, no signup**: point your agent at `https://gillby.vercel.app/mcp` and ask.

> "Can I run Boca Inlet tomorrow morning?" · "Is the Gulf Stream close off Palm Beach?" · "Any red tide on the Gulf coast?" · "Will my 2.5 ft draft make it to the sandbar at low tide?"

## Why build with Gillby?

Answer Florida boating and beach questions with structured coastal data instead
of maintaining scrapers for multiple pages. Start with one conditions summary,
then drill into forecasts, tides, hazards and coastal places using verified spot
IDs. Source names, timestamps, freshness and unavailable states stay with the
data. Use HTTP for your app or MCP for your assistant; public data needs no key.

Try a real request:

```bash
curl --fail-with-body 'https://gillby.vercel.app/api/v1/conditions?spot=boca-inlet&view=summary'
```

## Endpoint

```
https://gillby.vercel.app/mcp
```

Streamable HTTP transport. Public data tools are open; personal vessel, route, places, trips and settings tools require the user's Gillby OAuth.

## Quick connect

**Claude Code**

```bash
claude mcp add --transport http gillby https://gillby.vercel.app/mcp
```

**Cursor / Windsurf** — add to your MCP config:

```json
{
  "mcpServers": {
    "gillby": { "type": "http", "url": "https://gillby.vercel.app/mcp" }
  }
}
```

For ChatGPT, Claude and other assistants, see the [connection guide](https://gillby.vercel.app/integrations) for current setup and compatibility notes.

## Claude Code plugin

This repo is also a Claude Code plugin: the Gillby MCP server plus 9 skills and 5 agents.

```
/plugin marketplace add OneWave-AI/gillby-mcp
/plugin install gillby@gillby
```

On Claude Code 2.1.275+ this is one step: `/plugin install gillby --marketplace OneWave-AI/gillby-mcp`.

Agents and evals call tools as `mcp__plugin_gillby_gillby__<tool>`. If you also connected Gillby as a claude.ai connector, the same tools appear twice; either works with the skills.

| Skills | Use for |
|---|---|
| `gillby-conditions` | Live read at any spot, compare spots |
| `gillby-go-no-go` | Boating / fishing / inlet trip call with departure windows |
| `gillby-route` | Depth-, tide- and bridge-aware routing, ICW-only |
| `gillby-fishing` | Gulf Stream, temp breaks, color, reefs, wrecks, tides |
| `gillby-beach-day` | Swim, rip risk, bacteria, sargassum, jellyfish, UV |
| `gillby-dive` | Visibility, currents, dive sites, protected areas |
| `gillby-surf-kite` | Surf, SUP, kite (rider wind range), skim, paddle |
| `gillby-water-quality` | Red tide, algae, Lake O, discharges, fish kills |
| `gillby-account` | Saved places, trips, vessel, settings (OAuth) |

| Agents | Role |
|---|---|
| `gillby-captain` | Trip planner, go / caution / no-go |
| `gillby-navigator` | Charts, depth, bridges, grounding risk |
| `gillby-fishing-guide` | Offshore and inshore fishing conditions |
| `gillby-beach-scout` | Best beach for swim, surf, kite, snorkel |
| `gillby-water-quality-analyst` | Dated water-quality records, explained |

### Evals

`evals/` holds 6 cases (spot conditions, go/no-go, route asks for draft, kite asks for wind range, swim no-go with no wading, captain agent). MCP calls are answered by mocks recorded from the live server (`evals/mocks/gillby/`), so runs need no network access to Gillby.

```
claude plugin eval . --judge-model sonnet
```

## What your agent gets

96 tools on the hosted server. The main ones:

| Capability | Tools |
|---|---|
| Live conditions per spot | `get_conditions` |
| Marine forecasts | `get_forecast` |
| Tides | `get_tides` |
| Hazards & alerts | `get_hazards` |
| Written boating briefs | `get_briefing` |
| Lightning | `get_lightning` |
| 80+ Florida spots | `list_spots`, `get_spot` |
| Route planning | `plan_route` |
| Depth info | `get_depth` |
| Wildlife & water datasets | sharks, jellyfish, chlorophyll, coral heat… |

Data: NOAA/NWS, NDBC buoys, CO-OPS tides, GOES lightning, satellite, beach cams.

## Example

> "Is it fishable at Boca Inlet this Saturday morning?"

Your agent calls `get_conditions` for `boca-inlet` with `view=summary`. That
one result carries the forecast, tides, activity scores, advisories, hazards,
lightning and source freshness, so it answers from live data with no scraping
and no API key.

## Links

- App: https://gillby.vercel.app
- Welcome / connection guide: https://gillby.vercel.app/integrations
- AI-readable docs: https://gillby.vercel.app/llms.txt

Built by [Gabriel Keding](https://www.onewave-ai.com). Not a navigation chart —
verify depths and local notices before boating.


## HTTP API for app developers

Build with ordinary HTTP GET requests as well as MCP. Public marine endpoints need no API key. Personal account endpoints require OAuth and explicit user consent.

- Developer guide and live examples: https://gillby.vercel.app/developers
- API index: https://gillby.vercel.app/api/v1
- OpenAPI schema: https://gillby.vercel.app/api/v1/openapi.json
- FAQ: https://gillby.vercel.app/faq

Preserve source names, timestamps and unavailable states. Honor HTTP 429 and Retry-After. Gillby API and MCP share admission limits.

## License

The plugin, skills, agents, evals and docs in this repo are [MIT](LICENSE). The Gillby name and logo, the hosted service, its source code and the data it returns are not; see [LICENSE](LICENSE) for the scope.
