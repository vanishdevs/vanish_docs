---
description: >-
  The settings are split across a few files in the shared folder. This page maps
  out each one and explains the parts worth knowing.
---

# Configuration

| File | What it controls |
| --- | --- |
| `shared/config.lua` | How selling works: command, keybind, autosell, picker, sale, UI, leaderboard |
| `shared/config_drugs.lua` | Which items are drugs, their price and per-sale limit |
| `shared/config_ranks.lua` | The XP and rank progression |
| `shared/config_police.lua` | Police count requirements and price scaling |
| `shared/config_zones.lua` | The sell zones and any hot zones |
| `shared/config_peds.lua` | The list of NPC models used as buyers |

## shared/config.lua

### Command, keybind and autosell

```lua
command = {
    enabled = true,
    aliases = { 'selldrugs', 'selldrug', 'trap' }, -- Any of these starts a sale
    help = 'Sell your current drugs for money',
    mode = 'single', -- 'single' = one sale per press, 'autosell' = keep selling until you run out
},

keybind = {
    enabled = false,
    defaultKey = 'O', -- Players can rebind this in F8
},

autosell = {
    delay = 3000,        -- Wait between auto-sale retries (ms)
    notifyToggle = true, -- Notify when autosell turns on or off
},
```

With `mode = 'autosell'`, the first press starts selling and the script keeps moving the same drug until the player runs out, leaves the zone, dies, gets in a vehicle, or runs the command again to stop.

### Picker and sale

```lua
picker = {
    mode = 'auto', -- 'auto' = first drug owned, 'pick' = a menu to choose from
    inventoryImagePath = 'nui://ox_inventory/web/images/%s.png',
},

sale = {
    interaction = 'keybind', -- 'keybind' = press a key, 'target' = ox_target option on the NPC
    keybindControl = 38,     -- Control id when using keybind (38 = E)
    keybindLabel = 'E',      -- Label shown in the prompt
    maxDistance = 10.0,      -- NPC walks off if the player gets this far away
    rejectChance = 30,       -- % chance the NPC turns the deal down
    preventWithWeapon = true,-- Block selling with a weapon drawn
},
```

### Police alerts, UI and leaderboard

```lua
policeAlerts = {
    notifyOnSuccess = false, -- Alert police on a successful sale
    notifyOnReject = true,   -- Alert police on a rejected sale
},

ui = {
    staticMode = false,           -- true = XP bar always on screen, false = fades after each sale
    position = 'top-center',      -- Where the XP bar sits
    backgroundColor = 'rgba(0, 0, 0, 0.7)',
},

leaderboard = {
    enableCommand = true,
    commandName = 'drugleaderboard',
    defaultLimit = 20,
    logo = '',           -- A URL or nui:// path, or leave empty for the text fallback
    logoFallback = 'DR',
    title = 'Drug Leaderboard',
    subtitle = 'Top dealers in the city',
},

hotZones = {
    notifyOnEntry  = true, -- Tell the player when a sale starts in a hot zone
    notifyOnPayout = true, -- Show the boost amount when the sale pays out
},
```

## shared/config_drugs.lua

Each key is an item name from your inventory.

```lua
return {
    ['coke'] = { price = 275, max = 10, account = 'black_money' },
    ['weed'] = { price = 275, max = 10, account = 'black_money' },
}
```

| Field | What it is |
| --- | --- |
| `price` | Price paid per unit, before any multipliers. |
| `max` | Most units that can be sold in a single transaction. |
| `account` | Which account the payment lands in, for example `black_money` or `money`. |

## shared/config_ranks.lua

```lua
return {
    xpBasedOnAmount = true, -- XP per sale = units sold. false = use the rank's xpIncrease
    levels = {
        { name = 'cornerboy', label = 'Cornerboy',  color = '#FF0000', level = 1, xpIncrease = 1, rankXP = 2500,  multiplier = 1.0  },
        { name = 'trapper',   label = 'Trapper',    color = '#00FF00', level = 2, xpIncrease = 1, rankXP = 5500,  multiplier = 1.01 },
        { name = 'trapstar',  label = 'Trapstar',   color = '#0000FF', level = 3, xpIncrease = 1, rankXP = 14000, multiplier = 1.04 },
        { name = 'dopemover', label = 'Dope Mover', color = '#FF00FF', level = 4, xpIncrease = 1, rankXP = 20000, multiplier = 1.07 },
    },
}
```

The lowest `level` is where new players start, and ranking up walks the list in order. `rankXP` is the XP needed to leave that rank, and `multiplier` is the earnings boost the player gets while at it. You can rename, reorder, add or remove ranks freely; the database follows whatever you set here.

## shared/config_police.lua

```lua
return {
    requiredAmount = 0, -- Minimum cops online before anyone can sell
    notifyChances = { success = 10, reject = 20 }, -- % chance a dispatch alert fires
    jobs = { 'police' }, -- Jobs counted toward the cop total
    multiplier = {
        ['1'] = 1.0,
        ['2'] = 1.3,
        ['3'] = 1.4,
        ['4'] = 1.5,
    },
}
```

The `multiplier` table keys are the number of cops online. The more police on duty, the higher the payout, which rewards players for dealing under pressure.

## shared/config_zones.lua

Players must be standing inside a zone for a sale to work.

```lua
return {
    testblock = {
        coords = vector3(1377.2616, -741.1827, 67.2328),
        radius = 30,
        -- hot = { multiplier = 1.5, bonus = 250 }, -- makes this a hot zone
    },
}
```

Add a `hot` table to any zone to turn it into a hot zone:

* `multiplier` is applied after the rank and police multipliers (1.5 = 50% more),
* `bonus` is a flat amount added on top of the final price.

## shared/config_peds.lua

A plain list of ped models the script picks from for the NPC buyers. Add or remove models to fit the look you want.
