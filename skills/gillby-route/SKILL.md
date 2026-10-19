---
name: gillby-route
description: Plan a depth-, tide- and bridge-aware boat route in Florida with Gillby — chart depths along the path, under-keel clearance, bridge vertical clearance at predicted tide, ICW-only routing and weather along the way. Use when the user asks how to get from one marina, ramp, inlet or sandbar to another, whether their boat can make it at low tide, if a bridge will clear, or wants an ICW route.
---

# Route planning

`plan_route` is chart-aware: it samples depths across the boat's beam, applies tide, finds bridges and restrictions, checks bridge clearance against predicted MHW where data allows, and samples weather along the line.

## Required inputs

- **from / to**: spot IDs or `lat,lon`. Use `list_spots`, `search_pois`, `find_marinas`, `find_boat_ramps`, `find_fuel_docks`, `find_inlets` or `find_anchorages` to resolve names. Never guess coordinates.
- **Vessel**: a confirmed library boat ID (`list_boats`) or measured `draft` (ft). Also ask for `airDraft` if any bridge is likely. `beam` improves the depth sampling. Signed-in users can call `plan_my_route`, which uses their saved vessel via `get_my_vessel`.
- **Never invent vessel dimensions.** If missing, ask.

Optional: `time` (ISO departure, drives tide and bridge clearance), `speed` (kt), `ukc` (under-keel clearance margin, ft), `bridgeMargin` (ft), `via` (up to eight ordered stops, semicolon separated), `corridor: "icw"` for a hard ICW-only constraint.

Cold charts can take up to ~110 seconds. Tell the user it is working if it is slow.

## Reading the result

Preserve and report: `passageStatus`, `bridgeClearance` per bridge, `weatherSamples`, minimum depth and where, reached offsets for `via` stops (stops can snap up to 300 m), and every source gap.

- `corridor: "icw"` with unknown channel or depth coverage returns **no verified route**. Say that; do not fill the gap with a straight line.
- A depth route is not an overall safe-passage verdict. Shoaling moves, charts lag, and point depths cannot prove a corridor is clear.
- Bridge clearance at predicted tide is a prediction. Tell the user to read the clearance boards.
- For bascule/swing bridges, check `list_bridges` for opening schedules when the vessel needs an opening.
- Use `get_depth` for a single-point check and `list_sandbars` for shallow-water hazards near the line. `get_navigation_notices` for Local Notice to Mariners items (closures, missing aids).

## Output

Lead with a passage summary (verified / partially verified / no verified route), then minimum depth vs draft + UKC, bridges with clearance vs air draft, timing advice (e.g. "leave after 10 AM so the Boca flats carry 1.2 ft more water"), weather along the route, and gaps.
