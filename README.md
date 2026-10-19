![Gillby](assets/gillby-logo.svg)

![Gillby icon](.claude-plugin/icon.png)

<h1 align="center">Gillby MCP</h1>

<p align="center">
  <b>Florida marine conditions for AI agents.</b> Tides, waves, wind, marine forecasts, hazards, red tide, rip currents, depth-aware boat routing and 84 verified coastal spots, through one remote MCP server and a public HTTP API.
</p>

<p align="center">
  <a href="https://registry.modelcontextprotocol.io/v0.1/servers?search=io.github.OneWave-AI%2Fgillby"><img alt="MCP Registry" src="https://img.shields.io/badge/MCP%20Registry-io.github.OneWave--AI%2Fgillby-20394D"></a>
  <a href="#claude-code-plugin"><img alt="Claude Code plugin" src="https://img.shields.io/badge/Claude%20Code-plugin-D97A3C"></a>
  <a href="https://www.gillby.com/api/v1/openapi.json"><img alt="OpenAPI" src="https://img.shields.io/badge/OpenAPI-v1-62A6C3"></a>
  <a href="LICENSE"><img alt="License: MIT" src="https://img.shields.io/badge/license-MIT-20394D"></a>
</p>

Connect any MCP-compatible agent (Claude, ChatGPT, Cursor, Windsurf, Claude Code) to **Gillby**, a guide to Florida water. Public data needs **no API key, no login, no signup**: point your agent at `https://www.gillby.com/mcp` and ask.

<p align="center">
  <img src="docs/gillby-panel.webp" alt="The Gillby coastal panel inside an AI assistant, showing spots across Florida on a map" width="420">
</p>

## Why Gillby exists

I'm newer to boating. I'm not from Florida, but I've been down here for years. My wife said it first: it was way too hard to get a good read on the weather without checking five different things, and when we asked an AI, it didn't have good answers on its own.

So I built Gillby. It's a hobby and a passion project: one map and one clock for a day on Florida water, plus this MCP server so your assistant can answer from real water data instead of guessing. More in [Why I built Gillby](https://www.gillby.com/blog/why-i-built-gillby).

> "Can I run Boca Inlet tomorrow morning?" · "Is the Gulf Stream close off Palm Beach?" · "Any red tide on the Gulf coast?" · "Will my 2.5 ft draft make it to the sandbar at low tide?"

## What makes it different

- **One map, one clock.** Wind, waves, tide, radar and lightning line up on the same timeline, per spot.
- **Chop for your boat.** Ride comfort accounts for boat size and hull, so a 21-foot center console gets a different answer than a big sportfish.
- **Bridge clearance at today's tide.** 97 bridges, with charted clearance adjusted for the NOAA tide prediction and the nearest live water-level gauge when it passes checks.
- **Calibrated rain and thunder chances.** Probabilities are recalibrated against airport observations, so a stated chance matches how often it actually rains.
- **A 16-day outlook with honest confidence.** Ensemble ranges and a trend against normal. From day 10 on, every day is labeled "Low confidence: trend only."
- **Sources stay attached.** Every answer carries source names, observation times, freshness and anything unavailable.

## Endpoint

```
https://www.gillby.com/mcp
```

Streamable HTTP transport. Public data tools are open; personal vessel, route, places, trips and settings tools require the user's Gillby OAuth.

## Quick connect

**Claude (claude.ai and Claude Desktop)**

1. Open Settings, then Connectors. Choose Add connector, then Add custom connector.
2. Enter `Gillby` as the name and paste the server URL above. Choose no sign-in for public conditions, then Add and Connect.
3. Start a new conversation with Gillby enabled and approve tool use when asked.

Connectors added on claude.ai also show up in Claude Desktop.

**ChatGPT**

1. Open Plugins (some accounts call it Apps or Connectors). Choose Add, then Add custom MCP server. Workspaces may need admin access.
2. Enter `Gillby` as the name, paste the server URL, and choose No authentication for public conditions. Create it, then Connect.
3. Start a new chat and select Gillby from your tools.

**Claude Code**

```bash
claude mcp add --transport http gillby https://www.gillby.com/mcp
```

**Cursor / Windsurf** — add to your MCP config:

```json
{
  "mcpServers": {
    "gillby": { "type": "http", "url": "https://www.gillby.com/mcp" }
  }
}
```

Try it: *"Use Gillby to check current conditions at Boca Inlet. Include source freshness and any unavailable data."* Full steps and troubleshooting: [connection guide](https://www.gillby.com/integrations).

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

## Tools

96 tools on the hosted server, generated from the live [tool reference](https://www.gillby.com/llms-full.txt). Start with `list_spots` and `get_conditions` with `view=summary`; that one result carries the forecast, tides, scores, advisories, hazards, lightning and source freshness.

<details>
<summary>All 96 tools by category</summary>

**Start here** (7)

| Tool | What it does |
|---|---|
| `get_health` | Worker heartbeat, stale feed families, government dataset status and the GLM lightning feed |
| `list_spots` | Every named Florida spot the app scores, with its lat/lon, kind, region and the NOAA stations used for it |
| `get_spot` | A single spot record plus now.scores (go/caution/nogo per activity for the current hour) when the worker has scored it |
| `get_conditions` | Full bundle for a spot: waves, wind, tide, water temp, activity scores with reasons, 7-day hourly forecast, tides, alerts, hazards, lightning and a briefing |
| `get_briefing` | Short headline and body written from live data, with numbers validated against the inputs |
| `get_source_status` | Fresh, stale, unavailable and QC-rejected sources used by spot conditions |
| `get_data_coverage` | Published dataset inventory and verified spot coverage; distinguishes unavailable data |

**Forecast and conditions** (22)

| Tool | What it does |
|---|---|
| `get_forecast` | Hourly forecast for a spot or lat/lon: waves, swell split, wind, gusts, tide, current, rain and thunder chance, and activity scores |
| `get_water_temperature` | Water temperature for a verified spot over the next hours |
| `get_waves` | Wave height, period and swell for a verified spot over the next hours |
| `get_wind` | Wind and gusts for a verified spot over the next hours |
| `get_rain` | Rain probability and accumulation for a verified spot over the next hours |
| `get_thunder_forecast` | Thunderstorm forecast for a verified spot over the next hours |
| `get_uv_forecast` | UV forecast for a verified spot over the next hours |
| `get_visibility` | Atmospheric visibility for a verified spot over the next hours |
| `get_air_temperature` | Air temperature for a verified spot over the next hours |
| `get_pressure` | Air pressure for a verified spot over the next hours |
| `get_cloud_cover` | Cloud cover for a verified spot over the next hours |
| `get_chop` | Boat ride and chop (use get_chop_history for past trends) for a verified spot over the next hours |
| `get_currents` | Predicted inlet currents for a verified spot over the next hours |
| `get_gulf_stream` | Approximate Gulf Stream edge for a verified spot |
| `get_surf_forecast` | NWS surf zone forecast for a verified spot |
| `get_chop_history` | Gillby's native 0-10 chop series for up to 31 days, daily mean/max, coverage and 7-day trends |
| `get_history` | Read stored observations or forecast history for source and var |
| `get_observations` | Actual buoy and station observations for a spot with timestamps |
| `get_wave_models` | Published wave-model series and model spread for a spot; absent models stay unavailable |
| `get_departure_windows` | Published best windows filtered by activity |
| `get_activity_scores` | Hourly published activity scores with reasons and vetoes |
| `compare_spots` | Compare current conditions for up to five verified comma-separated spot IDs |

**Tides, hazards and alerts** (9)

| Tool | What it does |
|---|---|
| `get_tides` | High and low tides (ft above MLLW, NOAA CO-OPS), hourly tide curve, inlet current, sunrise/sunset and moon phase |
| `get_hazards` | Active NWS marine alerts plus rip risk, red tide, bacteria, sargassum, water clarity, king tide, Gulf Stream distance and lightning |
| `get_lightning` | GOES-19 GLM lightning |
| `get_navigation_notices` | USCG local notices to mariners from the published agency dataset, with source and availability |
| `get_derelict_vessels` | Reported derelict vessels from the published agency dataset, with source and availability |
| `get_park_alerts` | Coastal national park alerts and closures from the published agency dataset, with source and availability |
| `get_rip_risk` | Rip current risk for a verified spot |
| `get_king_tide` | King tide forecast for a verified spot |
| `get_marine_alerts` | Active and upcoming alerts for a spot, retaining start/end times and source links |

**Routes, depth and boats** (7)

| Tool | What it does |
|---|---|
| `list_boats` | Boats with published specs (LOA, beam, draft, deadrise, weight, hull type, fuel, cruise speed) and their sources |
| `get_boat` | A single boat record from the library |
| `plan_route` | Chart-aware water routing between from/to spot IDs or lat,lon |
| `get_depth` | NOAA ENC chart depth and optional DEM depth at a Florida coordinate, with charted hazards, channels and restriction zones |
| `list_sandbars` | Search published sandbars by name or west,south,east,north bbox |
| `list_bridges` | Search published bridges by name or west,south,east,north bbox |
| `check_route_conditions` | Weather and marine conditions at up to five ordered spot IDs |

**Places and cams** (16)

| Tool | What it does |
|---|---|
| `get_protected_areas` | Marine protected areas from the published agency dataset, with source and availability |
| `find_marinas` | Search published marinas by name or west,south,east,north bbox |
| `find_fuel_docks` | Search published fuel docks by name or west,south,east,north bbox |
| `find_boat_ramps` | Search published boat ramps by name or west,south,east,north bbox |
| `find_anchorages` | Search published anchorages by name or west,south,east,north bbox |
| `find_reefs` | Search published reefs by name or west,south,east,north bbox |
| `find_wrecks` | Search published wrecks by name or west,south,east,north bbox |
| `find_dive_sites` | Search published dive sites by name or west,south,east,north bbox |
| `find_beaches` | Search published beaches by name or west,south,east,north bbox |
| `find_waterfront_dining` | Search published waterfront restaurants by name or west,south,east,north bbox |
| `find_inlets` | Search published inlets by name or west,south,east,north bbox |
| `find_piers` | Search published piers by name or west,south,east,north bbox |
| `search_pois` | Search public marine POIs |
| `get_poi` | Look up a published coastal POI by its exact ID |
| `list_cams` | Live source links and credits near a spot or coordinate; archived imagery may be unavailable |
| `get_camera_image` | Fetch a public camera JPEG with source timestamp metadata for the Gillby panel |

**Water quality and marine life** (17)

| Tool | What it does |
|---|---|
| `get_shark_research` | Published Florida shark research occurrences with observation dates, location uncertainty, source credits and licenses |
| `get_jellyfish_observations` | Published Florida jellyfish, box jellyfish and man-of-war observations from OBIS and iNaturalist, with dates and credits |
| `get_canal_discharges` | Canal discharge and freshwater releases from the published agency dataset, with source and availability |
| `get_lake_okeechobee` | Lake Okeechobee stage and releases from the published agency dataset, with source and availability |
| `get_usgs_water` | USGS flow, salinity and water temperature from the published agency dataset, with source and availability |
| `get_chlorophyll` | Satellite chlorophyll and water color from the published agency dataset, with source and availability |
| `get_temperature_fronts` | Satellite sea temperature fronts from the published agency dataset, with source and availability |
| `get_coral_heat` | Coral heat stress and bleaching outlook from the published agency dataset, with source and availability |
| `get_algae_reports` | Reported algae samples from the published agency dataset, with source and availability |
| `get_fish_kills` | Reported fish kills from the published agency dataset, with source and availability |
| `get_pollution_notices` | Pollution incident notices from the published agency dataset, with source and availability |
| `get_coastal_sensors` | SECOORA coastal sensor observations from the published agency dataset, with source and availability |
| `get_sargassum_outlook` | Sargassum research product availability from the published agency dataset, with source and availability |
| `get_beach_bacteria` | Beach bacteria samples for a verified spot |
| `get_red_tide` | Red tide samples for a verified spot |
| `get_water_clarity` | Satellite water clarity for a verified spot |
| `get_sargassum` | Local sargassum risk for a verified spot |

**Datasets** (2)

| Tool | What it does |
|---|---|
| `list_datasets` | Index of government datasets (discharges, Lake O, USGS, CoastWatch, coral heat, USCG notices and more) with status and last-good time |
| `get_dataset` | The latest payload of one dataset from list_datasets, including sharks and jellyfish |

**Your account (OAuth)** (16)

| Tool | What it does |
|---|---|
| `get_my_vessel` | Requires OAuth login to your Gillby account |
| `plan_my_route` | Requires OAuth login |
| `list_my_places` | Read your saved places and favorites |
| `get_my_place` | Read one saved place |
| `save_my_place (writes to your account)` | Save a place or favorite |
| `update_my_place (writes to your account)` | Update specified saved-place fields |
| `delete_my_place (writes to your account)` | Delete a saved place |
| `list_my_trips` | Read your saved trips |
| `get_my_trip` | Read one saved trip |
| `save_my_trip (writes to your account)` | Save a trip |
| `update_my_trip (writes to your account)` | Update specified trip fields; this does not recheck route safety |
| `delete_my_trip (writes to your account)` | Delete a saved trip |
| `get_my_settings` | Read saved vessel, preferences and notification settings |
| `update_my_settings (writes to your account)` | Update only specified vessel or preference fields |
| `get_my_notification_settings` | Read notification preferences and actual delivery availability |
| `update_my_notification_settings (writes to your account)` | Save notification preferences |

</details>

## HTTP API

Everything is also available over ordinary HTTP GET. Public endpoints need no key; account endpoints need OAuth and the user's consent.

```bash
curl --fail-with-body 'https://www.gillby.com/api/v1/conditions?spot=boca-inlet&view=summary'
```

- Developer guide, examples, error codes and versioning: https://www.gillby.com/developers
- OpenAPI schema: https://www.gillby.com/api/v1/openapi.json
- AI-readable docs: [llms.txt](https://www.gillby.com/llms.txt) and [llms-full.txt](https://www.gillby.com/llms-full.txt)

**Rate limits.** About 60 data requests per minute per client, shared between the API and MCP. Over the limit you get HTTP 429 with `Retry-After`; honor it and back off. A request without a spot returns 400 `spot_required`.

## Data and credits

Data comes from NOAA and NWS (forecasts, marine zones, alerts), NDBC buoys, NOAA CO-OPS tides and water levels, NOAA GOES-19 lightning, Open-Meteo and ECMWF/GFS models, NOAA CoastWatch, USGS, FWC, FDEP, FDOH, USCG and other public sources. Each response names the sources it used.

Gillby is for planning, not navigation. Check official charts, notices and conditions on the water.

## License

The plugin, skills, agents, evals and docs in this repo are [MIT](LICENSE). The Gillby name and logo, the hosted service, its source code and the data it returns are not; see [LICENSE](LICENSE) for the scope. Changes are listed in the [changelog](CHANGELOG.md).

---

Built by Gabe Keding.
