---
description: >-
  The turf config broken into sections: global rules, rewards and dropoffs,
  respawn, zones, commands, the editor, player settings and logging.
---

# Configuration

Everything lives in `shared/config.lua`, with logging in `bridge/logging/config.lua`. Zones can be set here or built live with the in-game editor.

## Global rules

```lua
global = {
    globalCoolDownSeconds = 200, -- Cooldown between turfs server-wide
    singleActive = false,        -- true = only one turf anywhere at a time
    zoneCooldownSeconds = 10,    -- Per-zone cooldown after a turf ends (0 = off)
    announceMode = 'chat',       -- 'off', 'chat' or 'notify'

    minStartingMembers = 2,      -- Online gang members needed to start (1 = off)
    warningWindowSeconds = 0,    -- Broadcast a warning this many seconds before (0 = off)

    -- Optional time-of-day window (server-local hours)
    schedule = {
        enabled = false,
        startHour = 20,          -- 8pm
        endHour = 2,             -- to 2am (spans midnight)
    },
},
```

## Rewards

The `rewards` block decides what the winning gang gets and how it reaches them.

```lua
rewards = {
    -- 'inventory' = straight into the gang stash
    -- 'players'   = split among winning members standing in the zone
    -- 'dropoff'   = spawn a loot stash at the zone's dropoff point to collect
    distributionMode = 'inventory',
    dropoff = {
        mode = 'marker',          -- 'marker', 'ped' or 'prop' at the dropoff point
        interaction = 'target',   -- 'key' or 'target'
        duration = 300,           -- Seconds the dropoff stays before it despawns (0 = forever)
        restrictToWinner = true,  -- Only the winning gang can open it
        proximityCheckDistance = 5.0, -- Anti-cheat distance check when opening
        stash = { slots = 25, weight = 100000 },
        -- marker / ped / prop / target sub-blocks set the look and label
    },
},
```

{% hint style="info" %}
Each zone can override `distributionMode` and the dropoff details on its own entry, so one zone can drop loot on the ground while another pays straight into the stash.
{% endhint %}

The actual items a zone awards live on the zone itself (see [Zones](#zones) below).

## Respawn

What happens to players who die during a turf. Players still alive are left alone so winners can stay and loot.

```lua
respawn = {
    enabled = true,    -- Revive dead players in the zone when the turf ends
    instant = false,   -- true = respawn players the moment they die
    delay = 5,         -- Seconds before instant respawn (instant mode only)
    mode = 'location', -- 'location' = teleport to the spot below, 'death_spot' = revive where they fell
    location = {
        coords = vector3(-1539.9905, 197.5291, 57.6735),
        heading = 214.0,
    },
},
```

## Zones

Each zone is one piece of territory. Set them in the `zones` table, or build them live with `/turfeditor`.

```lua
zones = {
    {
        label = "Crack",
        captureTime = 10, -- Seconds to hold the point to win
        start  = { coords = vector3(-1649.99, 151.04, 62.16), scale = vector3(2.0, 2.0, 1.0) },
        center = { coords = vector3(-1649.99, 151.04, 62.16), scale = vector3(60.0, 60.0, 50.0) },
        blip = { enabled = true, sprite = 310, color = 1, scale = 0.8, shortRange = true, name = "Crack" },
        rewards = {
            -- { name, minAmount, maxAmount, chance (0-100) }
            { name = "black_money", minAmount = 3, maxAmount = 5, chance = 100 },
            { name = "water",       minAmount = 2, maxAmount = 5, chance = 20 },
        },
    },
}
```

| Field | What it is |
| --- | --- |
| `label` | The zone's name, used in messages and as its ID. |
| `captureTime` | How long, in seconds, a gang must hold the point to win. |
| `start` | The point players interact with to begin a turf. |
| `center` | The capture area itself, with its size. |
| `blip` | The map blip for the zone. |
| `rewards` | The item pool. Each item rolls its own `chance`, and the amount lands between `minAmount` and `maxAmount`. |

## Commands and permissions

Permissions use ox\_lib `restricted` values: `false` for anyone, or an ACE string or list.

```lua
commands = {
    leaderboard     = false,            -- Anyone can open the leaderboard
    turfsettings    = false,            -- Anyone can open their settings
    turfs           = { 'group.admin' },-- Admin panel
    turfsend        = { 'group.admin' },-- Force end a turf
    turfstart       = { 'group.admin' },-- Force start a turf
    turfextend      = { 'group.admin' },-- Extend a running turf
    turfeditor      = { 'group.admin' },-- Open the zone editor
    setturfganglogo = { 'group.admin' },-- Set a gang's logo
},
```

The leaderboard command name is set separately under `leaderboard.commandName` (default `turfleaderboard`).

## Leaderboard

```lua
leaderboard = {
    enabled = true,
    commandName = 'turfleaderboard',
    periods = {
        daily = 1,    -- Day windows for the period dropdown (<= 0 disables)
        weekly = 7,
        monthly = 30,
    },
},
```

## Interaction and markers

```lua
interaction = {
    key = 38,          -- Key to start a turf (38 = E)
    distance = 1.0,    -- Distance to show the prompt
    throttleMs = 5000, -- Anti-spam between start requests
},

marker = {
    start  = { type = 1,  color = { r = 255, g = 0, b = 0, a = 200 } },
    center = { type = 28, color = { r = 255, g = 0, b = 0, a = 200 } },
},
```

## Player settings

The `playerSettings.defaults` block is the starting point every new player gets, and the floor when someone resets with `/turfsettings`. Players save their own overrides on top. It covers the HUD theme and accent colour, blip visibility, notification position and mutes, the text-UI overlay positions, and the kill feed (position, scale, lifetime, max rows, filter and so on). The defaults are sensible, so you only need to touch this to change what new players start with.

## Editor caps

```lua
editor = {
    maxLabelLength = 64, -- Longest zone name the editor accepts
    maxRewards = 24,     -- Most reward entries per zone
    maxPeds = 12,        -- Most peds per zone
},
```

## Inventory icons

The reward picker pulls item icons from this base URL. The default points at ox\_inventory's image folder. Change it if you host icons elsewhere.

```lua
inventory = {
    imageBase = 'https://cfx-nui-ox_inventory/web/images/',
    imageExt = 'png',
},
```

## Logging

Set up logging in `bridge/logging/config.lua`. Turf events can go to Discord, Fivemanage, Fivemerr, ox\_lib or a custom handler. Everything is off by default, so enable the ones you want and add their credentials.
