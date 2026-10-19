---
name: gillby-surf-kite
description: Surf, kiteboard, wing, skimboard and SUP forecasts for Florida beaches with Gillby — swell height, period and direction, wind quality, surf zone forecasts, kite wind windows matched to the rider's own range, and activity scores for surf, sup-surf, kite, skim and paddle. Use when the user asks if it's surfable, when the swell arrives, if it's kiteable, or the best session window.
---

# Surf and kite

## Surf / SUP surf / skim-wave

1. `get_surf_forecast` for the beach (NWS surf zone breaking heights) and `get_waves` / `get_wave_models` for swell height, period and direction.
2. `get_activity_scores` with `activity` = `surf`, `sup-surf` or `skim-wave`.
3. Report swell (ft @ s from direction), breaking height, wind direction relative to the beach, and the best window.

Offshore wind (W on the Atlantic coast) grooms surf but pushes paddlers and SUPs out to sea. Mention drift risk whenever you recommend paddling out in offshore wind. Model sea height is not breaking surf height.

## Kiteboarding / winging

Call `get_conditions` with `kiteStyle` (`freeride`, `waves`, `foil`), `kiteExperience` (`beginner`, `independent`, `advanced`), and the **rider-supplied** `kiteMinWindKt` and `kiteMaxWindKt`. If the rider has not told you their wind range, ask. Never infer it.

Read the `kiteboarding` block for weather screening and gust spread. There is no overall kite score and launch status is explicitly unverified: never claim a launch is legal, open or patrolled. Onshore or side-onshore is the safe direction; flag offshore wind as dangerous for kiting.

## Skim (flatland) and paddle

Skim (flatland) suitability is unscored without shoreline observations. Do not substitute another activity's score. Paddle (flatwater) uses `activity: "paddle"`; note when current predictions are unavailable, because return conditions are then unverified.
