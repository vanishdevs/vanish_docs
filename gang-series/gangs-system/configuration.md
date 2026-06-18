---
description: >-
  Every config file the gang system ships with, what each block does, and the
  parts worth understanding before you go live.
---

# Configuration

The settings are split across a few files in the `shared` folder so each part is easy to find. Logging lives in its own bridge file.

| File | What it controls |
| --- | --- |
| `shared/config.lua` | Admin access, the HUD, management points and the default stash size |
| `shared/config_creation.lua` | Creation templates, creation cost and cooldowns, invite expiry |
| `shared/config_stash.lua` | Stash image path, size and per-rank withdrawal limits |
| `shared/config_health.lua` | The gang health score and its alerts |
| `shared/config_managements.lua` | Per-gang management locations (only when management mode is `multiple`) |
| `bridge/logging/config.lua` | Where gang events get logged |

## shared/config.lua

### Admin access

```lua
admin = {
    keybind = 'F9', -- Keybind to open the admin menu

    -- Permission groups per action. Set an action to false to allow everyone.
    permissions = {
        gangadmin = { 'group.god', 'group.admin', 'group.mod' },
        creategang = { 'group.god', 'group.admin', 'group.mod' },
        deletegang = { 'group.god', 'group.admin', 'group.mod' },
        setpasscode = { 'group.god', 'group.admin', 'group.mod' },
        -- ... one entry per admin command

        -- Player commands (set to false to allow all players)
        currentgang = false,
        acceptinvite = false,
        declineinvite = false,
        leavegang = false,
    },
},
```

Each admin action maps to a command and a list of groups allowed to run it. Player-facing commands like accepting an invite are set to `false` so anyone can use them.

### HUD

```lua
hud = {
    enabled = true,
    showOnlyInGang = true, -- Hide the HUD for players not in a gang
    defaults = {           -- Only used when showOnlyInGang = false
        gangName = 'Unwanted',
        rankName = 'Bum',
    },
    position = { bottom = '10px', right = '20px', top = 'auto', left = 'auto' },
    backgroundColor = 'rgba(51, 51, 51, 0.0)',
    boxShadow = '0 0 15px rgba(0, 0, 0, 0.0)',
},
```

### Management points

Where leaders go to manage their gang and open the stash.

```lua
management = {
    -- 'single' = one shared spot, 'multiple' = per-gang spots in config_managements.lua
    mode = 'single',

    -- Used when mode = 'single'
    location = {
        coords = vector3(-268.4806, -957.1339, 31.2231),
        heading = 208.3381,
    },

    interactionType = 'ped',     -- 'ped' for an NPC, 'marker' for a ground marker
    interactionDistance = 10.0,

    npc = { enabled = true, model = 'g_m_importexport_01', scenario = 'WORLD_HUMAN_CLIPBOARD' },
    marker = { enabled = false, type = 1, size = vec3(1.0, 1.0, 0.1), color = vec3(255, 255, 255) },

    notifications = {
        enabled = true,
        template = '<div style="...">💀 {0}</div>', -- Chat card for join/leave/kick
    },
},
```

{% hint style="info" %}
Set `mode = 'multiple'` to give each gang its own management spot. The locations then come from `shared/config_managements.lua` (covered below), and only that gang's leaders can use their point.
{% endhint %}

## shared/config_creation.lua

Controls the create-a-gang experience.

### Templates

Templates pre-fill a rank ladder when a gang is created. They show up in the admin create menu automatically.

```lua
templates = {
    ['street_gang'] = {
        label = 'Street Gang',
        description = '4 ranks - classic street hierarchy',
        ranks = {
            { name = 'recruit',    label = 'Recruit',    ranking = 1, icon = '🚸' },
            { name = 'soldier',    label = 'Soldier',    ranking = 2, icon = '🔫' },
            { name = 'lieutenant', label = 'Lieutenant', ranking = 3, icon = '⭐' },
            { name = 'leader',     label = 'Leader',     ranking = 4, icon = '👑' },
        },
        leadership_rank = 4,
    },
    -- motorcycle_club, mafia_family, cartel, and custom also ship by default
}
```

Add your own template by copying one of the entries and changing the ranks. `custom` is always available and creates an empty gang you add ranks to afterwards.

### Cost, cooldowns and invites

```lua
cooldown = {
    enabled = false,
    days = 7,         -- Per-creator cooldown between creating gangs
    cost = 0,         -- 0 = free, otherwise charges framework money
    account = 'bank', -- 'bank' or 'cash'
},

recreateCooldown = {
    enabled = false,
    hours = 24,       -- Locks a deleted gang's name for this long
},

invites = {
    expireHours = 24, -- Unaccepted invites expire after this many hours
},
```

The recreate lock stops an admin deleting and instantly remaking a gang to wipe its roster quietly.

## shared/config_stash.lua

The shared gang stash.

```lua
return {
    -- Item image URL template. {item} is swapped for the item name.
    imagePath = 'nui://ox_inventory/web/images/{item}.png',

    -- Default stash size, applied when a gang is created
    inventory = { slots = 50, weight = 100000 },

    -- Per-rank daily withdrawal caps over a rolling 24 hour window
    limits = {
        enabled = true,
        defaults = {
            { rank = 1, daily_value_cap = 5000,  daily_item_cap = 5  },
            { rank = 2, daily_value_cap = 15000, daily_item_cap = 15 },
            { rank = 3, daily_value_cap = 50000, daily_item_cap = 50 },
            { rank = 4, daily_value_cap = 0,     daily_item_cap = 0  },
        },
    },
}
```

The limits stop low ranks from emptying the stash. `daily_value_cap` caps the total value a rank can pull in 24 hours, and `daily_item_cap` caps the number of items. Set either to `0` for no limit at that rank. These defaults seed every new gang, and leaders can adjust them per gang from the management UI.

{% hint style="warning" %}
Withdrawal limits are enforced through ox\_inventory. If you change the image path for a different inventory, the limit enforcement still expects ox\_inventory's hooks.
{% endhint %}

## shared/config_health.lua

An optional 0 to 100 score that tells you how alive a gang is.

```lua
weights = {
    activeMembers  = 30, -- Share of members seen in the last 14 days
    rankBalance    = 20, -- Penalizes gangs that are all rank 1
    recruitment    = 15, -- Recruitment open plus recent applications
    recentActivity = 25, -- Activity in the last 7 days
    stashHealth    = 10, -- Stash has at least one item
},

alerts = {
    enabled       = false,
    sweepMinutes  = 60,  -- How often every gang is scored
    warningBelow  = 40,  -- Fire an alert when a gang drops below this
    cooldownHours = 12,  -- Stops a gang near the line from spamming alerts
},
```

The weights do not need to add up to 100, the score is normalized. With `alerts.enabled`, the system fires a `vanish_gangs:server:gangHealthAlert` event when a gang crosses below the warning line, so you can route it to staff.

## shared/config_managements.lua

Only used when `management.mode = 'multiple'`. Each entry ties a management point to one gang, and only that gang's leaders can use it.

```lua
return {
    {
        gang = 'ballas',
        coords = vector3(-232.1606, -974.2057, 29.2886),
        heading = 287.1345,
    },
    -- add one block per gang
}
```

## Logging

Set up logging in `bridge/logging/config.lua`. Gang events (creation, deletion, joins, leaves, rank changes and more) can be sent to Discord, Fivemanage, Fivemerr, ox\_lib or your own custom handler. Everything is off by default, so enable the services you want and add their credentials.
