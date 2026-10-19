---
name: gillby-account
description: Manage a signed-in Gillby user's saved places, trips, vessel, settings and notification preferences through the Gillby MCP account tools (OAuth). Use when the user asks to save a spot, save or update a trip, list their trips, check their saved boat, plan with "my boat", or change Gillby settings or alert preferences.
---

# Gillby account

Account tools read and write **only the caller's own** records after they sign in to Gillby through the connector's OAuth flow and give account consent. Public data tools need no login.

## Tools

- Places: `list_my_places`, `get_my_place`, `save_my_place`, `update_my_place`, `delete_my_place`
- Trips: `list_my_trips`, `get_my_trip`, `save_my_trip`, `update_my_trip`, `delete_my_trip`
- Vessel: `get_my_vessel`; routing with it: `plan_my_route`
- Settings: `get_my_settings`, `update_my_settings`, `get_my_notification_settings`, `update_my_notification_settings`

## Rules

- **requestId**: every new save gets a fresh unique `requestId` (8-80 chars, `[A-Za-z0-9_-]`). Reuse the *same* requestId when retrying the same save so it is not duplicated.
- **Ask for missing trip details** — name, from, to, departure time. Never invent a departure time, vessel or coordinates. Resolve places to lat/lon with `list_spots` / `search_pois` first.
- **Saving a trip does not assess safety.** Offer a gillby-go-no-go check alongside.
- **Destructive actions** (`delete_my_place`, `delete_my_trip`) require a clear request from the user for that specific record. Confirm the name before deleting.
- **Report a change only after the tool returns success.** If it fails, say so with the error.
- Notification preferences do **not** turn on push, email or AI-client delivery by themselves. Do not promise alerts.
- Do not invent app UI steps ("tap Settings > ...").
- If an account tool returns an auth error, tell the user to sign in to Gillby through their Claude connector settings.
