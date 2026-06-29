---
description: >-
  Player Robbing settings are split across four files in the shared and bridge
  folders. This page maps out each one and explains the parts worth knowing.
---

# Configuration

| File | What it controls |
| --- | --- |
| `shared/config.lua` | Global rules, input method, animations, notifications, gang rules, leaderboard |
| `shared/config_zones.lua` | Optional zones that restrict where robberies can take place |
| `shared/config_whitelistitems.lua` | Items that can never be stolen from a victim |
| `shared/config_blacklistplayers.lua` | Jobs and identifiers barred from robbing or being robbed |
| `bridge/logging/config.lua` | Logging adapters (Discord, FiveManage, FiveMerr, ox\_lib, custom) |

## shared/config.lua

### Admin permissions

```lua
admin = {
    toggleRobbery    = 'admin',   -- /togglerobbery
    clearCooldown    = 'admin',   -- /clearrobcooldown
    resetLeaderboard = 'admin',   -- /resetrobleaderboard
},
```

Each value is an ace string (or `false` to allow everyone). Players without the ace are refused when they run the command.

### Global rules

```lua
global = {
    interactionDistance  = 2.0,   -- max metres the robber may stand from the victim
    robberyDuration      = 5000,  -- progress bar duration (ms)
    cooldownTime         = 300,   -- seconds before the same robber can rob again (0 disables)
    victimCooldown       = 600,   -- seconds before the same victim can be robbed again (0 disables)
    requireWeapon        = true,  -- robber must be holding any weapon (not fists)
    requireTargetHandsUp = true,  -- victim must have hands raised before the robbery can start
    preventVehicleRobbery = true, -- robber cannot initiate from inside a vehicle
    preventDeadRobbery   = true,  -- dead robbers cannot rob; dead victims cannot be robbed
    policeJobs = { 'police', 'sheriff' }, -- jobs counted for the fallback blip dispatch alert
    blipDuration = 60,            -- seconds the fallback blip stays on the map
    openInventoryOnSuccess = true, -- open the victim's inventory after a successful robbery
},
```

Setting `openInventoryOnSuccess = false` turns the robbery into a pure RP mugging — the progress bar and notifications fire, but no inventory window opens and no items change hands.

### Time restriction

```lua
timeRestriction = {
    enabled   = false,
    startHour = 20,  -- inclusive, 0-23 (in-game hour)
    endHour   = 6,   -- exclusive, 0-23; wraps past midnight when startHour > endHour
},
```

When `enabled = true`, a robbery can only be started while the in-game GTA world clock falls within the defined window. The window wraps past midnight, so `startHour = 20, endHour = 6` allows robberies between 8 pm and 6 am. This is enforced as a client-side gate (the same layer as the weapon and hands-up checks) because the GTA clock only exists client-side.

### Notifications

```lua
notifications = {
    notifyVictim = true,          -- victim receives a "you are being robbed" notification
    notifyPolice = true,          -- sends a dispatch or fallback blip alert to on-duty police
    position = 'top-right',       -- ox_lib notify anchor for all script notifications
},
```

Valid `position` values: `top`, `top-right`, `top-left`, `bottom`, `bottom-right`, `bottom-left`, `center-right`, `center-left`.

### Rate limits

```lua
rateLimits = {
    canRob    = { max = 10, windowSec = 10 }, -- robbery eligibility checks
    completed = { max = 5,  windowSec = 10 }, -- robbery completion submissions
    cancelled = { max = 10, windowSec = 10 }, -- robbery cancellation reports
},
```

Per-player sliding-window rate limits on the server events players can fire. Each entry caps a player at `max` calls within a `windowSec` second window. Setting `max` or `windowSec` to `0`, or removing the entry, disables limiting for that action. These exist to prevent exploitation of the server callbacks — the defaults should not need changing on a normal server.

### Gang integration

```lua
gangs = {
    enabled             = true,
    membersOnly         = false, -- only players in a gang are allowed to rob
    allowRobOwnGang     = false, -- same-gang members cannot rob each other
    allowRobAlliedGangs = false, -- allied-gang members cannot rob each other
    gangCooldownReduction = 0,   -- % reduction applied to the robber cooldown for gang members (0 disables)
},
```

All gang checks are skipped when `enabled = false`. Gang data comes from the vanish\_gangs bridge in `bridge/gangs/server/`.

`gangCooldownReduction` is an integer 0–100. For example, `25` means a gang member waits 25% less time between robberies than a solo robber. The reduction is applied on top of `global.cooldownTime`.

### Input method

```lua
input = {
    method = 'target', -- 'keybind' | 'command' | 'target' | 'both' (keybind + command)

    keybind = {
        enabled      = true,
        key          = 'E',                 -- default key (players can rebind in FiveM settings)
        secondaryKey = 'LSHIFT',            -- modifier held alongside key
        description  = 'Rob nearby player',
    },

    command = {
        enabled    = true,
        name       = 'rob',
        description = 'Rob the nearest player',
        restricted = false,   -- false, an ace string, or an ace list
    },

    target = {
        enabled = true,
        label   = 'Rob Player',
        icon    = 'fa-solid fa-hand-holding',
    },
},
```

`method = 'both'` registers both the keybind and the command. `method = 'target'` uses only the ox\_target option on each player.

### Animations

```lua
animations = {
    robber = {
        dict = 'combat@aim_variations@arrest',
        clip = 'cop_med_arrest_01',
    },
    victim = {
        dict = 'random@mugging3',
        clip = 'handsup_standing_base',
        handsUpControl = 323,  -- control id to toggle the hands-up pose (323 = H)
    },
},
```

The victim animation is also what the script checks when `requireTargetHandsUp = true`. Players toggle their hands up by pressing `handsUpControl` (default H) on foot.

### Progress indicator

```lua
progress = {
    type         = 'bar',     -- 'bar' (lib.progressBar) or 'circle' (lib.progressCircle)
    position     = 'bottom',  -- circle only: 'middle' | 'bottom'
    useWhileDead = false,
    canCancel    = true,
    label        = 'Robbing person...',
    disable = {
        car    = true,
        combat = true,
        move   = true,
    },
},
```

`type = 'bar'` uses a standard progress bar across the bottom of the screen. `type = 'circle'` uses a circular indicator; `position` controls whether it sits in the `'middle'` or at the `'bottom'` of the screen.

### Leaderboard

```lua
leaderboard = {
    enabled       = true,
    commandName   = 'robleaderboard',
    enableCommand = true,

    keybind = {
        enabled     = false,
        key         = 'F7',
        description = 'Open Player Robbing leaderboard',
    },

    title         = 'Player Robbing Leaderboard',
    subtitle      = 'Top robbers in the city',
    logo          = '',           -- image URL; falls back to logoFallback when empty
    logoFallback  = 'RB',         -- initials shown when no logo is set

    itemImagePath = 'nui://ox_inventory/web/images/{item}.png',

    defaultMetric  = 'successful',  -- metric selected when the UI opens
    rankingMetric  = 'successful',  -- metric used to calculate player ranks
    defaultLimit   = 25,            -- rows per page
    maxLimit       = 100,           -- server-side cap on a single fetch

    retentionDays        = 0,   -- prune rows older than N days (0 keeps all rows)
    lootTrackingWindow   = 120, -- seconds after a robbery that taken items count toward stats
    cashItems = { 'money', 'cash', 'black_money' }, -- item names treated as cash in stats
},
```

Valid `rankingMetric` / `defaultMetric` values: `successful`, `failed`, `total`, `cash`, `items`, `last`.

`itemImagePath` supports `{item}`, `%s`, or a plain folder path — the UI swaps in the item name automatically.

---

## shared/config\_zones.lua

When `enabled = true`, a robbery may only be started while the robber is standing inside one of the defined zones. Both the client (instant feedback) and the server (authoritative check) enforce this. Leave `enabled = false` to allow robberies anywhere on the map.

```lua
return {
    enabled = false,

    zones = {
        { label = 'Downtown Vinewood',  coords = vec3(215.0, -865.0, 30.0),   radius = 150.0 },
        { label = 'Legion Square',      coords = vec3(195.0, -935.0, 30.0),   radius = 120.0 },
        { label = 'Vespucci Beach',     coords = vec3(-1223.0, -1490.0, 4.0), radius = 200.0 },
    },
}
```

Each zone is a sphere defined by a `coords` (vec3) and a `radius` in metres. `label` is used in logs and notifications only. Add as many zones as you need.

---

## shared/config\_whitelistitems.lua

Items listed here are protected and will never be taken from a victim, regardless of what is in their inventory.

```lua
return {
    enabled = true,  -- false -> no items are protected; everything becomes stealable

    items = {
        'id_card',
        'driver_license',
        'weaponlicense',
        'phone',
        'radio',
        'WEAPON_PISTOL',
        'WEAPON_COMBATPISTOL',
    },
}
```

Add any ox\_inventory item name to the list to protect it. Set `enabled = false` to disable the whitelist entirely.

---

## shared/config\_blacklistplayers.lua

Controls which jobs or specific players are barred from the Player Robbing system.

```lua
return {
    enabled = true,  -- false -> all blacklist checks are skipped

    jobs = {
        cannotBeRobbed = { 'police', 'sheriff', 'ambulance', 'mechanic' },
        cannotRob      = { 'police', 'sheriff', 'ambulance' },
    },

    identifiers = {},  -- licence:xxxx strings permanently barred from robbing
}
```

`cannotBeRobbed` protects a job from being targeted. `cannotRob` stops a job from initiating a robbery. Both lists accept any framework job name. Individual players can be barred by adding their `license:xxxx` identifier to `identifiers`.

---

## bridge/logging/config.lua

Player Robbing events can be forwarded to one or more logging services. Each service has an `enabled` flag and a per-event opt-in table.

```lua
discord = {
    enabled  = false,
    url      = '',          -- Discord webhook URL
    botName  = 'Player Robbing Log',
    avatarUrl = '',
    events = {
        robbery_success = true,
        robbery_failed  = true,
    },
    colors = {
        robbery_success = 0x2ECC71,
        robbery_failed  = 0xE74C3C,
        default         = 0xE63946,
    },
},
```

| Service | Key fields |
| --- | --- |
| `discord` | `url` — webhook URL |
| `fivemanage` | `datasetId` — dataset name on your FiveManage account |
| `fivemerr` | `url`, `apiKey` — endpoint and key from your FiveMerr dashboard |
| `ox_lib` | No extra fields; logs via ox\_lib's built-in logger |
| `custom` | `handler` — a Lua `function(event)` that receives `{ action, actor, target, details }` |

Set `enabled = true` on any service and fill in its required fields to activate it. Multiple services can run at the same time.
