---
name: gillby-beach-day
description: Check whether a Florida beach is good for swimming or a beach day using Gillby — rip current risk, surf height, beach bacteria advisories, red tide, sargassum, jellyfish and man-of-war sightings, UV, lightning and NWS beach hazards. Use when the user asks "can we swim at X", "is the beach good today", "any jellyfish", "is there seaweed", "red tide at Y", or wants a family beach-day plan.
---

# Beach day

## Calls

1. `get_conditions` (`view: "summary"`) for the beach. Swim score + reasons, rip risk, surf, bacteria, red tide and lightning are already in it.
2. Add only what is missing:
   - `get_rip_risk` — NWS rip current risk.
   - `get_beach_bacteria` — FDOH Healthy Beaches sampling. Report the sample date.
   - `get_sargassum` and `get_sargassum_outlook`.
   - `get_jellyfish_observations` for jellyfish and Portuguese man-of-war; `get_shark_research` only if asked about sharks.
   - `get_uv_forecast`, `get_air_temperature`, `get_thunder_forecast`.
   - `find_beaches` / `list_cams` to suggest an alternative or show a live camera.

## Rules

- High rip current risk or a swim **no-go** → say do not swim. Never follow a swim no-go with a suggestion to wade.
- Rip current risk is issued per surf zone, not per beach. Lifeguarded beaches are safer; tell users to swim near a lifeguard and check the flags.
- Bacteria advisories are from a dated sample; say "sampled Oct 14: advisory" not "the water is contaminated today".
- Jellyfish, man-of-war and shark records are **dated published observations**, not live tracking, warnings or abundance estimates. "No records" does not mean absent. Man-of-war are siphonophores, not true jellyfish. Never apply NOAA's Chesapeake sea-nettle forecast to Florida.
- Local lifeguard links in results are references, not ingested current warnings.
- Purple flags on Florida beaches mean dangerous marine life; mention flags rather than inferring them.

## Output

Verdict (swim yes / caution / no) → why → seaweed, critters, bacteria with dates → UV and storms → a better nearby beach if one scores higher.
