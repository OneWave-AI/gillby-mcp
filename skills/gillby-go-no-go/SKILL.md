---
name: gillby-go-no-go
description: Make a go / caution / no-go call for a Florida boating, fishing or inlet trip using Gillby — current scores, departure windows, advisories, lightning, wind trend and the user's boat. Use when the user asks "should I go out tomorrow", "can I run the inlet", "best time to leave", "is it worth taking the boat out this weekend", or wants a trip plan with a departure and return window.
---

# Go / no-go trip call

Produce a skipper-style decision with the evidence behind it. The user makes the final call.

## Gather (only what is missing)

Ask once, briefly, for anything you need and cannot infer:
- **Where**: launch spot and destination (offshore, reef, sandbar, inlet).
- **When**: date and rough departure/return time. Never invent a departure time.
- **Activity**: boating, fishing-offshore, fishing-inshore, sailing, dive, inlet, paddle.
- **Boat**: only if they want boat-specific comfort. Library ID via `list_boats`, or measurements. Signed-in users can use `get_my_vessel`.

## Calls

1. `get_conditions` (`view: "summary"`, plus `boat` if confirmed) for the launch spot.
2. `get_activity_scores` for the activity, `hours` covering the trip (up to 168).
3. `get_departure_windows` for the activity. No windows returned means none were published — not that every other hour is unsafe.
4. If the trip passes an inlet, run the same for the inlet spot with `activity: "inlet"`. Inlets are the highest-risk leg: outgoing tide against onshore swell stacks up steep waves. Check `get_currents` for the inlet if available.
5. Several stops: `check_route_conditions` with up to five ordered spot IDs (weather only — no depth or bridges; use the gillby-route skill for that).
6. Optional context: `get_thunder_forecast`, `get_lightning`, `get_marine_alerts`, `get_wind` trend.

## Decision rules

- Any active Small Craft Advisory, Gale Warning, Special Marine Warning or waterspout warning covering the trip window → **no-go** for small craft unless the user explicitly has a vessel and experience suited to it; say so plainly.
- Use Gillby's band for the activity as the baseline. Upgrade to no-go if the return window falls outside published windows or if wind is building through the trip.
- Lightning observed nearby or afternoon thunder risk in summer → recommend an earlier return, state the observed window.
- Low-confidence scores (`confidence: "low"`) → call them out and lean conservative.

## Output

```
Call: NO-GO tomorrow morning from Hillsboro Inlet
Why: Small Craft Advisory until 8 PM Sat; NE 22 kt gusting 28; seas 7 ft at 10 s; inlet scored nogo (onshore heavy swell)
Best window: Mon 7-11 AM (boating: go, seas 3 ft, wind E 10 kt)
Watch: wind veers SE after noon Mon; tide ebbs 9:40 AM, so expect a lumpy inlet on the way out
Data gaps: current prediction unavailable for Hillsboro
```

Keep it scannable. Advisory times and source gaps always stay in.
