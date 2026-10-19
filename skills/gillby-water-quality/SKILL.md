---
name: gillby-water-quality
description: Florida water-quality and environmental report with Gillby — red tide (FWC Karenia brevis samples), blue-green algae, Lake Okeechobee levels and releases, canal discharges, fish kills, chlorophyll, beach bacteria, pollution notices and USGS water data. Use when the user asks about red tide, algae blooms, Lake O discharges, why the water is brown, fish kills, or whether water is safe to be in or let a dog swim in.
---

# Water quality

## Calls (pick by question)

| Question | Tools |
|---|---|
| Red tide | `get_red_tide` (FWC samples, cell counts, dates) |
| Algae blooms (lakes, rivers, St. Lucie, Caloosahatchee) | `get_algae_reports` |
| Lake Okeechobee | `get_lake_okeechobee` (stage, releases) |
| Brown / tannic water after rain | `get_canal_discharges`, `get_usgs_water`, `get_rain` |
| Dead fish | `get_fish_kills` |
| Beach bacteria | `get_beach_bacteria` |
| Spills, sewage, closures | `get_pollution_notices` |
| Satellite bloom context | `get_chlorophyll` |
| Park / beach closures | `get_park_alerts` |

## Rules

- Every water-quality data point has a **sample date**. Lead with it. Old samples do not describe today.
- Red tide categories come from FWC cell counts; report the category and count, not a health verdict. Respiratory irritation is possible at low concentrations near the beach.
- Blue-green algae: when a bloom or toxin is reported, advise keeping people and pets out of the water and pointing them to FDOH advisories. Never declare water safe.
- Chlorophyll is satellite-derived and not species-specific. High chlorophyll is not proof of red tide.
- No samples or reports means no data, not clean water.
