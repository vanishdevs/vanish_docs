---
description: >-
  The settings broken into sections: the phone, the war rules, rewards, the UI,
  rank permissions, war zones and logging.
---

# Configuration

The main settings live in `shared/config.lua`. Rank permissions are in `shared/config_management.lua`, war zones in `shared/config_zones.lua`, and logging in `bridge/logging/config.lua`.

## Phone and admin

```lua
phone = {
    enabled = true,            -- Require an inventory item to open the phone
    itemName = 'burnerphone',  -- The item name (must exist in your inventory)
    leaderOnly = false,        -- If true, only gang leaders can open the phone
    command = 'gangphone',     -- Command to open the phone (false to disable)
},

admin = {
    permissions = {
        forceEndWar = { 'group.god', 'group.admin' }, -- /endwar <warId>
        listWars = { 'group.god', 'group.admin' },    -- /listwars
    }
},
```

## War rules

This block decides how wars start, run and end.

```lua
war = {
    defaultKillTarget = 10,   -- Default kills to win
    minKillTarget = 5,        -- Lowest a leader can set
    maxKillTarget = 50,       -- Highest a leader can set
    winBy2 = {
        enabled = false,      -- Must lead by 2 once the target is reached
        lead = 2,
    },

    minWager = 1000,          -- Smallest wager allowed
    maxWager = 1000000,       -- Largest wager allowed
    defaultWager = 10000,

    -- Where the wager money comes from
    wagerFunding = {
        mode = 'leader',      -- 'leader', 'stash', or 'leader_or_stash'
        preferStash = false,
        stashItem = 'money',  -- ox_inventory item used as gang vault money
    },

    maxSimultaneousWars = 3,  -- Server-wide cap on active wars
    maxWarsPerGang = 1,       -- Active wars per gang
    showOnlineGangsOnly = true,

    maxDuration = 3600,       -- War time limit in seconds (0 = no limit)
    challengeExpiration = 1800, -- Pending challenges auto-decline after this

    cooldown = {
        enabled = true,
        duration = 600,       -- Cooldown after a war before the gang can fight again
    },

    killDetection = {
        distance = 100.0,           -- Max killer to victim distance to count a kill
        downedGracePeriod = 120,    -- Stops a downed player being double counted
        crutch = {
            enabled = true,         -- Use wasabi_crutch state if it is running
            countKills = true,      -- Whether killing a crutched player counts
        },
    },
},
```

{% hint style="info" %}
**When the time limit runs out:** the gang with more kills wins and gets the payout. If the kills are tied it is a draw and both wagers are returned.
{% endhint %}

## Rewards

```lua
rewards = {
    winnerMultiplier = 2.0,       -- $10,000 wager x 2.0 = $20,000 payout
    distributionMode = 'leader',  -- 'leader', 'members' or 'stash'
    memberDistribution = 'equal', -- When mode is 'members'
    bonuses = {
        enabled = false,
        flawlessVictory = 5000,   -- Won with zero deaths
        comeback = 2000,          -- Won after being down by 3+
    },
},
```

## UI

Theme colours, controls and the on-screen displays.

```lua
ui = {
    closeKey = 'ESC',
    openKey = 'F4',           -- Keybind to toggle the phone ('' to disable)
    theme = {
        primary = '#670000',
        accent = '#c90000',
        background = '#1a1a1a',
        text = '#ffffff',
    },
    warHud = {
        enabled = true,       -- On-screen war status during a war
        showKillFeed = true,
    },
    killFeed = { enabled = true },
},
```

The `markers` and `phoneProp` blocks control the ally/enemy markers shown during a war and the phone prop animation. They are safe to leave on the defaults.

## Rank permissions

`shared/config_management.lua` decides what each gang rank is allowed to do. Set a shared default for all gangs, and override per gang where you want something different.

```lua
rankPermissions = {
    default = {
        [1] = { invite = false, kick = false, promote = false, demote = false, acceptWar = false, challengeWar = false, setLogo = false },
        [2] = { invite = true,  kick = true,  promote = false, demote = false, acceptWar = false, challengeWar = true,  setLogo = false },
        [3] = { invite = true,  kick = true,  promote = true,  demote = true,  acceptWar = true,  challengeWar = true,  setLogo = true  },
    },
    gangs = {
        -- ["ballas"] = { [5] = { acceptWar = true, challengeWar = true, setLogo = true } },
    },
},
```

The numbers are rank levels, with the highest number being leadership. `challengeWar` lets a rank start a war, `acceptWar` lets them accept one, and `setLogo` lets them change the gang logo shown in the app.

## War zones

By default a war kill counts anywhere on the map. To restrict war kills to set areas, enable zones in `shared/config_zones.lua`:

```lua
return {
    enabled = false,  -- true = only kills inside these areas count
    locations = {
        -- { coords = vector3(0.0, 0.0, 0.0), radius = 150.0 },
    }
}
```

## Logging

Set up logging in `bridge/logging/config.lua`. War and challenge events can be sent to Discord, Fivemanage, Fivemerr, ox\_lib or your own custom handler. Every service is off by default, so enable the ones you want and add their credentials.

## Seasonal stats

```lua
statsSeasonal = {
    enabled = false,        -- Wipe the leaderboard on a schedule
    resetInterval = 2592000, -- Seconds between resets (30 days)
},
```
