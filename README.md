# Gillby MCP

Connect any MCP-compatible AI agent to **Gillby** — the AI guide to Florida
water — and give it live marine conditions, forecasts, tides, hazards, and
80+ Florida boating/fishing/surf spots.

**No API key. No login. No signup.** Point your agent at the endpoint and go.

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

## What your agent gets

| Capability | Tools |
|---|---|
| Live conditions per spot | `get_conditions` |
| Marine forecasts | `get_forecast` |
| Tides | `get_tides` |
| Hazards & alerts | `get_hazards` |
| Written boating briefs | `get_briefing` |
| Lightning | `get_lightning` |
| 84 Florida spots | `list_spots`, `get_spot` |
| Route planning | `plan_route` |
| Depth info | `get_depth` |
| Wildlife & water datasets | sharks, jellyfish, chlorophyll, coral heat… |

Data: NOAA/NWS, NDBC buoys, CO-OPS tides, GOES lightning, satellite, beach cams.

## Example

> "Is it fishable at Boca Inlet this Saturday morning?"

Your agent calls `get_conditions` + `get_forecast` + `get_tides` for
`boca-inlet` and answers from live data — no scraping, no API keys.

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
