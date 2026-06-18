---
description: >-
  The main config, the performance tuning options, the map calibration and the
  logging setup, all explained.
---

# Configuration

## shared/config.lua

```lua
return {
    debug = false,

    -- The app as it appears on the phone
    app = {
        identifier = 'livelocation',
        name = 'Find My Contacts',
        description = 'Share live location with phone contacts',
        icon = 'https://.../icon.png',
    },

    location = {
        -- How often a watched player's position is pushed out. Lower is smoother but uses more bandwidth.
        pushIntervalMs = 5000,
        -- Past this age the app shows "last seen" instead of "updated".
        staleAfterMs = 60000,
        -- Don't push an update unless the player has moved at least this many units.
        minMoveDistance = 5.0,
    },

    request = {
        -- How long before the same pair can re-send a request.
        cooldownMs = 30000,
        -- Most outstanding requests a single number can have out at once (anti-spam).
        maxPending = 20,
    },

    blip = {
        enabled = true,
        sprite = 280,
        color = 5,
        scale = 0.85,
    },

    theme = {
        primary = '#0A84FF',
        accent = '#30D158',
        -- ... the rest of the app's colours
    },
}
```

{% hint style="info" %}
**`pushIntervalMs` is the main dial for performance vs smoothness.** 5000 (five seconds) is a good balance. Drop it for tighter tracking on small servers, raise it if you run a high population server and want to keep the load down.
{% endhint %}

### Performance tuning

The `presence` block controls how the app keeps track of who is online. The defaults are tuned to stay near idle on busy servers, so most owners never need to touch them.

```lua
presence = {
    -- 0 disables the background online scan entirely. Online dots still refresh from
    -- app activity and disconnects. Set above 0 to bring back periodic reconciles.
    reconcileMs = 0,
    batchSize = 8,
    batchWaitMs = 250,
    numberCacheMs = 60000,
    -- ... see the file for the rest
}
```

### Map calibration

The `map` block lines the app's map up with the GTA world. It uses the same tile set as lb-phone's own Maps app, so the defaults are correct out of the box. Only change these if you run a custom map.

```lua
map = {
    tileServer = 'https://.../map-tiles/gtav/main/{layer}/{z}/{x}/{y}.jpg',
    defaultCenter = { 200, -800 }, -- Downtown Los Santos
    defaultZoom = 2,
    minZoom = 2,
    maxZoom = 7,
}
```

## Logging

Share activity can be logged to one or more services. Open `bridge/logging/config.lua` and enable whichever you want. Every service is off by default.

| Service | How to enable |
| --- | --- |
| Discord | Set `enabled = true` and paste a webhook `url`. |
| Fivemanage | Set `enabled = true` and set your `datasetId`. |
| ox\_lib | Set `enabled = true` (uses ox\_lib's logging config). |
| Custom | Set `enabled = true` and provide your own `handler` function. |

Each service can pick which events it logs:

```lua
events = {
    share_requested = true,
    share_accepted  = true,
    share_declined  = true,
    share_revoked   = true,
},
```
