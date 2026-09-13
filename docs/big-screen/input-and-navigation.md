---
icon: lucide/gamepad-2
---

# Input and Navigation

Every button press routes to exactly one consumer. This page covers the routing order, the gates before it, and how focus moves and restores across rows.

## Routing Order

Every command goes to exactly one consumer in order of their position in the hierarchy:

1. Top Modal
2. Overlay Screen
3. Dashboard

## Focus Movement

The navigation controller owns row state explicitly, because a game card stays logically focused even while the options or profile row is active. It never touches visuals. It emits focus and scroll requests that the window fulfills with real focus and scroll calls posted after layout.

Moving between games and options preserves columns through two fixed maps, clamped to actual counts: each option owns a pair of games going down, each option jumps to its group head going up. Stepping onto the profile row remembers the game index and clears the visible selection without losing it.

Stepping back down returns to the same card. Right off the profile row advances to the next game with wraparound from last to first. Left on the profile row does nothing. An empty library pins navigation to the options row.

## Focus Restoration

Keyboard and gamepad activation restore option focus when an overlay closes. Mouse activation on an option card activates without leaving selection, so clicks never corrupt row state. Right-click on a game card selects it first, scrolling Library to it when needed, then opens the game details, exactly mirroring Details.

Initial selection after boot is the first recent game, or the options row when the library is empty. After a game session it restores by game ID with fallback to the first game, scrolling after layout so the card is visible.

## Dashboard Rows

Up and Down move between profile, games, and options rows per the rules above. Left and Right move within the current row. Activate launches the focused game, opens the focused option screen or quits, or opens the profile picker from the profile row. Details opens the game details for the focused card.

## Header Contents

Profile identity applied at boot and on every profile change. Clock in the configured format with fixed widths against jitter, refreshed every second. Network preferring wireless over ethernet over disconnected, polled on an interval, keeping last state on failure.

Controller battery with charging awareness, warning when disconnected. Profile rotation across versions is display only and never changes the active profile.
