# Hurricane data: API and MCP

Gillby provides active NHC tropical cyclones and finalized NOAA NHC HURDAT2 best tracks for **2000–2025**: **973 cyclones, 27,812 track records, 26 completed seasons**. Coverage includes the Atlantic (`al`) and eastern (`ep`) and central (`cp`) Pacific. Both tools are public reads with no API key or account.

- [Hurricane page](https://www.gillby.com/hurricanes)
- [OpenAPI](https://www.gillby.com/api/v1/openapi.json)
- [Exact MCP input schemas](https://www.gillby.com/integrations/tools#get_hurricane_history)
- [NHC archive and format references](https://www.nhc.noaa.gov/data/#hurdat)

## Active storms

```sh
curl --fail-with-body 'https://www.gillby.com/api/v1/tools/get_hurricanes?basin=all'
```

`get_hurricanes` accepts optional `basin`: `all` (default), `al`, `ep`, or `cp`. It returns active cyclone position, classification, maximum sustained wind in knots, pressure in millibars, movement direction **toward** a bearing and speed in knots, advisory time and official advisory/forecast/cone product links. Movement differs from marine wind directions, which are normally FROM. The feed is cached for five minutes. Preserve `status`, retrieval times, per-storm `advisoryStale` and response freshness metadata; a failed refresh can return last-good data with `status: "stale"`. An unavailable feed returns HTTP 503 with `status: "unavailable"` and `activeCount: null`.

## Historical storms, first observation to last

```sh
# All recorded points along Hurricane Katrina's 2005 track
curl --fail-with-body 'https://www.gillby.com/api/v1/tools/get_hurricane_history?id=al122005&includeTrack=true'

# Atlantic hurricanes with a recorded landfall during 2000–2005
curl --fail-with-body 'https://www.gillby.com/api/v1/tools/get_hurricane_history?basin=al&startYear=2000&endYear=2005&hurricanesOnly=true&landfallOnly=true&limit=20&offset=0'
```

| Parameter | Default | Meaning |
|---|---|---|
| `basin` | `all` | `all`, `al`, `ep`, or `cp` |
| `startYear`, `endYear` | `2000`, `2025` | Inclusive season years; must be within archive coverage and start ≤ end |
| `name` | omitted | Case-insensitive substring, e.g. `Katrina`; names recur, so add years or use an ID |
| `id` | omitted | Exact storm ID, e.g. `al122005` |
| `hurricanesOnly` | `false` | Only cyclones classified as hurricanes at some recorded point |
| `landfallOnly` | `false` | At least one NHC landfall flag, on any coastline |
| `includeTrack` | `false` | Include every recorded point for each returned storm |
| `limit` | `20` | 1–100 storms per page |
| `offset` | `0` | 0–10,000; use returned `nextOffset` until null |

The response retains `{data, meta}`. `data.coverage`, source archive URLs and SHA-256 hashes describe the bundled import. `total`, `limit`, `offset` and `nextOffset` describe pagination; `storms` contains the selected page. Each summary includes season, basin, first/last times, peak sustained wind, minimum reported pressure, hurricane/major-hurricane flags, landfall count and track point count.

With `includeTrack=true`, each storm's `track` runs from its **first recorded observation to its last recorded observation**, including special-event records between regular six-hourly observations. This is the available recorded lifecycle, not a continuous real-time trace or a claim of observations before genesis/after dissipation. Points retain UTC `at`, `lat`, `lon`, cyclone `status`, sustained `windKt`, `pressureMb`, `recordIdentifier` and `landfall`. Wind radii are in nautical miles for 34/50/64 kt, in **NE, SE, SW, NW** quadrant order. Missing measurements are `null`, not zero. Records include tropical storms, depressions and other lifecycle stages unless filtered. Landfall flags do not specifically mean US or Florida landfall, and a storm can have multiple landfalls.

Finalized history ends in 2025. Ongoing 2026 advisories use `get_hurricanes`. Historical best tracks are retrospective analyses, not forecasts, damage estimates or future-risk predictions. The app displays current storm details and NHC map links; it does not yet plot historical tracks on its map. An empty active list does not rule out developing systems or local hazards. Forecast cones are not impact boundaries.

## MCP calls

Connect to `https://www.gillby.com/mcp` using Streamable HTTP. Tool arguments retain the `query` object:

```json
{"name":"get_hurricanes","arguments":{"query":{"basin":"al"}}}
```

```json
{"name":"get_hurricane_history","arguments":{"query":{"id":"al122005","includeTrack":true}}}
```

Full JSON-RPC example:

```sh
curl --fail-with-body 'https://www.gillby.com/mcp' \
  -H 'Content-Type: application/json' \
  -H 'Accept: application/json, text/event-stream' \
  --data '{"jsonrpc":"2.0","id":1,"method":"tools/call","params":{"name":"get_hurricane_history","arguments":{"query":{"id":"al122005","includeTrack":true}}}}'
```

## SDK and CLI

Use the SDK's generic request method or the CLI's public GET command:

```js
import { Gillby } from '@onewave-ai/gillby';
const gillby = new Gillby();
const { data, meta } = await gillby.request('/tools/get_hurricane_history', {
  query: { id: 'al122005', includeTrack: true },
});
console.log(data.storms, meta);
```

```sh
npx --no-install gillby request /tools/get_hurricanes --query basin=al
npx --no-install gillby request /tools/get_hurricane_history --query id=al122005 --query includeTrack=true
```

To refresh the bundled archive in the application repository, run `npx tsx scripts/hurricanes/ingest.ts`. It discovers official NHC archive links, imports completed seasons since 2000 and records source hashes. Regenerate or review coverage labels when the latest completed season changes. Respect returned rate limits and `Retry-After`.
