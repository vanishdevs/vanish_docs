---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

```lua
return {
    debug = true,

    -- Global turf settings
    global = {
        globalCoolDownSeconds = 200,      -- Global cooldown between turfs (seconds)
        singleActive = false,             -- true: only one active turf globally; false: multiple (one per zone)
        zoneCooldownSeconds = 10,         -- Per-zone cooldown after a turf ends (seconds, 0 = disabled)
        announceMode = 'chat',            -- 'off' | 'chat' | 'notify'
        gangProvider = 'vanish_gangs',    -- 'vanish_gangs' | 'rcore_gangs' (compat soon, not available)
    },

    leaderboard = {
        enabled = true,
        commandName = 'leaderboard',      -- command to open the UI
    },

    -- Command permissions (ox_lib `restricted` values). Use false, an ACE string, or an ACE list.
    commands = {
        leaderboard = false,
        turfs = { 'group.admin' },
        turfsend = { 'group.admin' },
        setganglogo = { 'group.admin' },
    },

    zones = {
        {
            label = "Crack",
            captureTime = 10,
            start = { coords = vector3(-1649.9982, 151.0411, 62.1637), scale = vector3(2.0, 2.0, 1.0) },
            center = { coords = vector3(-1649.9982, 151.0411, 62.1637), scale = vector3(60.0, 60.0, 50.0) },
            blip = { enabled = true, sprite = 310, color = 1, scale = 0.8, shortRange = true, name = "Crack" },
            rewards = {                   -- Each: { name: string, minAmount: number, maxAmount: number, chance: 0-100 }
                { name = "black_money", minAmount = 3, maxAmount = 5, chance = 100 },
                { name = "water", minAmount = 2, maxAmount = 5, chance = 20 }
            }
        },
    },

    marker = {
        start = { type = 1,  color = { r = 255, g = 0, b = 0, a = 200 } },
        center = { type = 28, color = { r = 255, g = 0, b = 0, a = 200 } },
    }
}
```
