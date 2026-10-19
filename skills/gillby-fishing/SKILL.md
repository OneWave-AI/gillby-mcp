---
name: gillby-fishing
description: Build a Florida fishing report with Gillby — offshore Gulf Stream edge, temperature breaks, water color and chlorophyll, reefs and wrecks, tides and moving water for inshore, plus the go/no-go sea state. Use when the user asks where to fish, whether the Gulf Stream is close, where the color change or temp break is, what reefs or wrecks are nearby, or wants an inshore or offshore fishing plan.
---

# Fishing report

Gillby does not predict fish. It reports the water conditions anglers use to choose where to look. Say that if the user asks "where are the fish".

## Offshore

1. Sea state first: `get_conditions` (`view: "summary"`) for the nearest offshore spot (`list_spots`, kind `offshore`) and `get_activity_scores` with `activity: "fishing-offshore"`. If it is no-go, lead with that.
2. `get_gulf_stream` — distance and axis relative to the coast. Off southeast Florida the edge can sit just a few miles out; that is the headline offshore.
3. `get_temperature_fronts` — temperature breaks. `get_chlorophyll` and `get_water_clarity` — color change between green inshore water and blue stream water.
4. `get_sargassum` — weed lines hold bait and pelagics, but heavy mats foul gear.
5. Structure: `find_wrecks`, `find_reefs` near the run.
6. `get_protected_areas` for any marine sanctuary / no-take / SPA zones on the route. Do not tell the user what they may keep; point them to FWC regulations.

## Inshore

1. `get_activity_scores` with `activity: "fishing-inshore"`.
2. `get_tides` and `get_currents` — moving water windows. Report times of the turns, not just heights.
3. `get_water_temperature`, `get_water_clarity`, `get_canal_discharges` and `get_red_tide` (fresh discharge and red tide both shut down a bite and matter for handling fish).
4. Shore anglers: `find_piers`.

## Output

```
Offshore (Off Boca): no-go today, SCA through 8 PM; Monday looks fishable
Gulf Stream edge: ~X mi offshore (source, time)
Temp break: 78 -> 81 F along ~26.3N
Color: green-to-blue line ~X mi out per chlorophyll (satellite date)
Structure on the way: [wrecks/reefs]
Inshore: incoming 6:10-11:50 AM at Hillsboro; no canal discharges reported
```

Only numbers that came from tool results. Keep satellite product dates — chlorophyll and clarity are often a day or more old.
