---
description: >-
  All four config files broken down: the core queue behaviour, the waiting card,
  Discord credentials and logging.
---

# Configuration

| File | What it controls |
| --- | --- |
| `config/config.lua` | Core queue behaviour, priority and Discord rules |
| `config/config_card.lua` | The card players see while they wait |
| `config/config_discord.lua` | Your bot token and guild ID |
| `config/config_logging.lua` | Where queue events get logged |

## config/config.lua

```lua
return {
    debug = false,
    consoleInfo = true,         -- Print join/leave/admit info to the console
    serverName = 'My Server',   -- Used in webhooks and logging
    maxPlayers = 64,            -- Fallback if sv_maxclients is not set
    alwaysShowQueue = false,    -- Force everyone through the queue, even with open slots
    minimumQueueTime = 0,       -- Minimum seconds a player waits (0 = instant when a slot is free)

    -- Hold players briefly after a restart before letting them in
    joinDelay = {
        enabled  = true,
        duration = 30,
    },

    cooldown = 5,               -- Minimum seconds between connection attempts per player
    displayQueueInHostName = false, -- Prefix the hostname with [Queue: X] when players are waiting

    -- Players who disconnect get temporary priority to rejoin quickly
    grace = {
        enabled  = true,
        duration = 300,
        priority = 100,
    },

    -- Store priority points in the database
    database = {
        enabled = true,
    },

    -- Permission groups for each admin command
    admin = {
        addPriority    = { 'group.god', 'group.admin' },
        removePriority = { 'group.god', 'group.admin' },
        viewQueue      = { 'group.god', 'group.admin', 'group.mod' },
        clearQueue     = { 'group.god', 'group.admin' },
    },

    -- Discord integration (token and guild ID live in config_discord.lua)
    discord = {
        enabled            = false,
        requireIdentifier  = false, -- Kick players with no linked Discord account
        requireMembership  = false, -- Kick players who are not in your Discord
        whitelist = {
            enabled = false,
            roleId  = '',           -- Only this role may connect
        },
        roles = {
            -- ['ROLE_ID'] = { priority = 100, label = 'Staff' },
            -- ['ROLE_ID'] = { priority = 50,  label = 'VIP' },
        },
    },

    -- Priority point settings
    priority = {
        default       = 10,    -- Base points every player starts with
        stackPriority = true,  -- true = add every source together, false = use the highest only
        categories = {
            database = 'Database',
            discord  = 'Discord Role',
            grace    = 'Grace Period',
        },
    },
}
```

### How priority works

Every player starts with `priority.default` points. On top of that they can earn points from the database, from their Discord roles and from the grace period after a disconnect. `stackPriority` decides how those add up:

* **`true`**: every source is added together. A VIP (50) who just disconnected (100 grace) sits at 160 plus the base.
* **`false`**: only the single highest source counts.

Players are admitted highest priority first, so the more points someone has, the sooner they get in.

{% hint style="info" %}
`alwaysShowQueue = false` means players only see the queue when the server is actually full. Set it to true if you want everyone to pass through the card, even when there is room.
{% endhint %}

## config/config_card.lua

This is the card players look at while they wait. Set a banner image, an accent colour, and the buttons and pages shown.

```lua
return {
    bannerImage = 'https://.../banner.png',  -- Replaces the title block when set
    iconImage = 'https://.../icon.png',
    title = 'Server Queue',
    subtitle = 'You are waiting in line to join the server',
    waitingMessage = 'Join our Discord or visit our store while you wait!',
    accentColor = '#0a84ff',
    maxPlayersToDisplay = 10,  -- How many players show on the queue list page

    buttons = {
        store   = { enabled = true, label = 'Store',   url = 'https://store.yourserver.com', style = 'default' },
        discord = { enabled = true, label = 'Discord', url = 'https://discord.gg/yourserver', style = 'default' },
    },

    navigation = {
        queueList    = { label = 'Queue List',  style = 'default' },
        priorityList = { label = 'My Priority', style = 'default' },
        back         = { label = 'Back',        style = 'default' },
    },
}
```

Button and navigation `style` can be `default` (outlined), `positive` (green filled) or `destructive` (red filled).

## config/config_discord.lua

Your private Discord credentials. The bot needs the **Server Members Intent** turned on.

```lua
return {
    botToken = '',  -- From https://discord.com/developers/applications
    guildId  = '',  -- Right click your server, Copy Server ID
}
```

## config/config_logging.lua

Send queue events to one or more services. All off by default.

| Service | What to set |
| --- | --- |
| Discord | `enabled = true` and a webhook `url` |
| Fivemanage | `enabled = true` and your `datasetId` (needs the fmsdk resource) |
| Fivemerr | `enabled = true` and your `apiKey` |
| ox\_lib | `enabled = true` |
| Custom | `enabled = true` and your own `handler` function |

Each service picks which events it logs:

```lua
events = {
    queue_join      = true,
    queue_leave     = true,
    queue_admit     = true,
    priority_add    = true,
    priority_remove = true,
    denied          = true,
},
```
