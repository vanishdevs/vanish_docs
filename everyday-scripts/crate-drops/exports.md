---
description: >-
  Exports for hooking crate drops into your own resources, plus the admin
  commands that ship with the script.
---

# Exports

## Server

### CreateCrateDrop

Spawn a drop from your own code. Pass coordinates to place it, or leave them out for a random location from your config.

```lua
local success, crateId = exports.vanish_cratedrops:CreateCrateDrop(x, y, z, forced, tierName)
```

| Argument | Type | Notes |
| --- | --- | --- |
| `x, y, z` | number / nil | Where to drop it. Pass `nil` for a random configured location. |
| `forced` | boolean | `true` ignores the minimum player count and concurrent drop limit. |
| `tierName` | string / nil | `'common'`, `'uncommon'`, `'rare'`, `'epic'` or `'legendary'`. `nil` rolls a tier by chance. |

Returns: `success` (boolean) and `crateId` (number) when a drop was created.

```lua
-- Random drop, rolled tier
exports.vanish_cratedrops:CreateCrateDrop()

-- Forced legendary drop at a set of coordinates
exports.vanish_cratedrops:CreateCrateDrop(1865.0, 3680.0, 33.0, true, 'legendary')
```

### RemoveCrate

Remove an active crate by its ID.

```lua
exports.vanish_cratedrops:RemoveCrate(crateId)
```

### GetActiveCrates

Get the list of crates that are currently live.

```lua
local crates = exports.vanish_cratedrops:GetActiveCrates()
```

Returns: a table of the active drops, keyed by crate ID.

## Client

### useSupplySignal

Triggers the supply signal flow for the player: it validates the use server side, plays the placement animation and calls in the plane. This is the export you wire your supply signal item to.

```lua
exports.vanish_cratedrops:useSupplySignal()
```

## Commands

| Command | Argument | Description |
| --- | --- | --- |
| `/createdrop` | `tier` (optional) | Creates a drop at a random configured location. |
| `/createdrophere` | `tier` (optional) | Creates a drop at your current position. |

The `tier` argument accepts `common`, `uncommon`, `rare`, `epic` or `legendary`. Leave it off to roll a random tier. Access to each command is controlled by the `admin` block in `shared/config.lua`.
