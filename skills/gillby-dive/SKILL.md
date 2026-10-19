---
name: gillby-dive
description: Plan a Florida dive or snorkel with Gillby — visibility estimate, water clarity, seas, currents including Gulf Stream set, water temperature, dive sites, reefs, wrecks, coral heat stress and protected areas. Use when the user asks if diving or snorkeling is good, what the vis is, which reef or wreck to dive, or whether the current is ripping.
---

# Dive and snorkel

## Calls

1. `get_activity_scores` with `activity: "dive"` for the spot, and `get_conditions` (`view: "summary"`) for sea state and advisories.
2. `get_visibility` and `get_water_clarity` — Gillby's visibility index and satellite clarity. State the product date.
3. `get_currents` and `get_gulf_stream` — strong northward set off southeast Florida makes drift diving the norm; tell the user if it is strong enough to matter.
4. `get_water_temperature` for wetsuit advice; `get_coral_heat` for bleaching stress context.
5. Sites: `find_dive_sites`, `find_reefs`, `find_wrecks`. `get_protected_areas` for sanctuary rules and SPAs.
6. `get_marine_alerts` and lightning before recommending any trip.

## Rules

- Visibility numbers are estimates from indices and satellite, not diver reports. Say "estimated".
- Seas over the dive score's tolerance or any Small Craft Advisory → no boat dive.
- Shore snorkel from a beach also needs the gillby-beach-day rip-current check.
- Never invent site depths. Use what `get_poi` / `find_dive_sites` return.
