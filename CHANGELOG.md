# Changelog

Changes to the hosted Gillby MCP server and HTTP API that affect agents and integrations. API v1 changes are additive; anything breaking gets a new version and at least 90 days of notice.

## 2026-10-19

- **New canonical URL.** The server is now listed at `https://www.gillby.com/mcp`. The old `https://gillby.vercel.app/mcp` keeps working, and account sign-in works on either host.

## 2026-10-18

- **Missing spot returns 400.** Forecast, tide and hazard requests without a spot now return `400 spot_required` ("Pass ?spot=<id>") instead of a 503.
- **Full tool reference.** Added [llms-full.txt](https://www.gillby.com/llms-full.txt) with every tool, its description and parameters. [llms.txt](https://www.gillby.com/llms.txt) now lists all 96 tools by category.
- **Calibrated rain and thunder chances.** `precipChance` is now calibrated against airport observations where the calibrated model beats the raw forecast on held-out data. The original value is kept as `precipChanceRaw`. New hourly fields: `rain3hChance`, `rainLikely`, `thunder3hChance`.
- **Wind and wave ranges.** Hourly `ranges` give 10th to 90th percentile wind and wave height.
- **Gust correction.** Gusts are corrected for a known low bias in the first 48 to 72 hours.
- **16-day outlook.** Spot conditions include `outlook[]`: daily wind, gust, rain and wave ranges with a trend against normal, and confidence that drops with lead time. From day 10 on it reads "Low confidence: trend only."
- **Storm nowcast.** Spot conditions include `stormNowcast`, built from recent GOES-19 lightning near the spot. Any arrival time is labeled low confidence.
- **Live bridge clearance on the hosted server.** Bridge clearance now applies the residual from the nearest live NOAA water-level gauge that passes checks, not the tide prediction alone. When no gauge passes, the result says it is prediction only.
- **Developer docs.** Real example responses, rate limits, error codes, a versioning policy and attribution terms at [/developers](https://www.gillby.com/developers).
