---
description: >-
  Every config file the script ships with and what each block controls. The
  comments in the files cover the fine detail, this page gives you the map.
---

# Configuration

The script splits its settings across a few files in the `shared` folder so each part stays easy to find.

| File | What it controls |
| --- | --- |
| `shared/config.lua` | Global behaviour: drop frequency, timings, plane, crate, blips, sounds, zones |
| `shared/config_tiers.lua` | The five loot tiers and their loot tables |
| `shared/config_locations.lua` | The list of predefined random drop locations |
| `server/webhooks_config.lua` | Discord webhook logging |

## shared/config.lua

```lua
return {
    debug = false,

    -- Admin groups with access to commands
    admin = {
        createdrop = { 'group.god', 'group.admin', 'group.mod' },
        createdrophere = { 'group.god', 'group.admin', 'group.mod' },
    },

    -- Maximum number of concurrent active drops (0 = unlimited)
    maxConcurrentDrops = 5,

    -- Minimum players required for automatic drops
    minimumPlayers = 1,

    -- Gang integration (requires vanish_gangs)
    gangs = {
        enabled = true,
        notifyMembers = true, -- Notify gang members when crate drops
    },

    -- Chat notifications
    chat = {
        enabled = true,
        backgroundColor = '255, 0, 0', -- RGB format
        textColor = '#ffffff',
    },

    -- Notification settings
    notifications = {
        position = 'top-right',
        duration = 5000,
    },

    -- Combat zone settings
    zones = {
        enabled = true, -- Set to false to disable combat zones
        radius = 150.0,
        showNotifications = false, -- Show enter/exit notifications
    },

    -- Automatic drop system
    automaticDrops = {
        enabled = true,
        intervalMinutes = 30, -- Time between automatic drops
    },

    -- Drop timing settings
    timings = {
        planeDelay = 30, -- Seconds after blip creation before plane arrives
        unlockDelay = 300, -- Seconds before crate can be unlocked
        totalDuration = 3600, -- Seconds before crate despawns
        cleanupDelay = 300, -- Seconds before empty crate is removed
    },

    -- Supply signal security settings
    supplySignal = {
        itemName = 'supply_signal', -- Item name to validate
        cooldown = 300, -- Cooldown before a player can use another signal
        validateItem = true, -- Verify player has the item before allowing drop
    },

    -- Coordinate validation settings
    coordinateValidation = {
        enabled = true,
        minZ = -50.0, -- Prevents underground spawns
        maxZ = 1000.0,
        maxDistanceFromPlayer = 10.0, -- Max distance from player when using signal
    },

    -- Plane settings
    plane = {
        model = `titan`,
        speed = 120.0,
        height = 500.0,
        spawnDistance = 2000, -- Distance from drop point where plane spawns
        dropTriggerDistance = 350, -- Distance from drop point when plane releases crate
        blip = {
            enabled = true,
            sprite = 307,
            color1 = 6,
            color2 = 3,
            scale = 1.0,
            pulseInterval = 500,
        },
    },

    -- Crate settings (model, interaction distance, fall speed, particles,
    -- flare, blip, stash size, sounds and the open animation)
    crate = {
        model = `prop_drop_crate_01_set2`,
        interactionDistance = 3.5,
        spawnHeight = 150,
        -- ... see the file for the full block
        stash = {
            slots = 20,
            maxWeight = 100000,
            prefix = 'vanish_crate_',
        },
    },
}
```

{% hint style="info" %}
**Timings work as a sequence.** `planeDelay` is the wait before the plane shows up, `unlockDelay` is how long after landing before anyone can crack the crate open, and `totalDuration` is the full lifespan before it despawns. `cleanupDelay` only kicks in once a crate is emptied early.
{% endhint %}

## shared/config_tiers.lua

Each tier defines how good the loot is and how it looks on the map. Set `spawnChance` per tier to control how often it shows up on automatic drops. Anything left as `nil` falls back to the matching value in `config.lua`.

```lua
common = {
    label = 'Common Supply Crate',
    spawnChance = 0.70, -- Chance for automatic drops
    minItems = 3,
    maxItems = 5,
    blipColor = 0,
    particleColor = { r = 255, g = 255, b = 255 }, -- Smoke colour
    skillcheck = {
        enabled = false, -- Require an ox_lib skillcheck before opening
        difficulty = { 'easy' },
        keys = { 'w', 'a', 's', 'd' },
    },
    lootItems = {
        { item = 'water', min = 1, max = 3, chance = 1.0 },
        { item = 'bread', min = 1, max = 3, chance = 0.8 },
        { item = 'bandage', min = 2, max = 5, chance = 0.6 },
    },
},
```

For each entry in `lootItems`:

* `item` is the inventory item name,
* `min` and `max` set the amount range that gets rolled,
* `chance` is the odds that item makes it into the crate at all (1.0 is always).

The crate rolls between `minItems` and `maxItems` from the table, so not every listed item appears in every crate.

{% hint style="warning" %}
The defaults ship with water, bread and bandages so the script runs on a fresh server. Replace them with real items before you go live, and make sure every item name exists in your inventory.
{% endhint %}

## shared/config_locations.lua

The pool of spots a random drop can land in. Each one has a name, world coordinates and a radius the drop can scatter inside.

```lua
return {
    { name = 'Sandy Shores',  coords = vector3(1865.0, 3680.0, 33.0),  zoneRadius = 150.0 },
    { name = 'Paleto Bay',    coords = vector3(-450.0, 6000.0, 31.0),  zoneRadius = 200.0 },
    { name = 'Mount Chiliad', coords = vector3(500.0, 5600.0, 800.0),  zoneRadius = 300.0 },
    -- add or remove locations to suit your map
}
```

## server/webhooks_config.lua

Discord logging. Off by default. Flip `enabled` to true and paste a webhook URL into each event you want logged. Leave any URL as `false` to skip that event.

```lua
configWebhooks = {
    enabled = false,
    urls = {
        dropCreated = false,
        dropOpened = false,
        dropLooted = false,
        dropExpired = false,
        adminActions = false,
        supplySignal = false,
    },
    footer = {
        text = 'vanish_cratedrops',
        icon_url = '',
    },
    tierColors = {
        common = 9807270,
        uncommon = 5763719,
        rare = 3447003,
        epic = 10181046,
        legendary = 15105570,
    },
}
```
