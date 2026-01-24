---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

config.lua

```lua
return {
    debug = false,

    -- Global jam settings
    global = {
        enabled = true,
        defaultChance = 20.0,             -- Default jam chance percentage (0-100)
        checkInterval = 0,                -- Milliseconds between jam checks (0 = every shot)
        jamDuration = 1000,              -- How long before you can clear the jam (ms)
        clearJamKey = 51,                 -- Key to clear jam (https://docs.fivem.net/docs/game-references/controls/)
    },

    -- Notification settings
    notifications = {
        onJam = true,                     -- Show notification when weapon jams
        onClear = true,                   -- Show notification when jam is cleared
        type = 'error',                   -- Notification type for jam
        position = 'top-right',           -- Notification position
    },

    -- Visual/Audio effects when jammed
    effects = {
        screenShake = true,               -- Screen shake on jam
        shakeIntensity = 0.5,             -- Shake intensity (0.0-1.0)
        sound = true,                     -- Play sound on jam
        soundName = 'WEAPON_PURCHASE',    -- Sound name
        soundSet = 'HUD_AMMO_SHOP_SOUNDSET', -- Sound set
    },

    -- Clearing jam settings
    clearJam = {
        successChance = 25,               -- Chance to successfully clear jam per attempt (0-100)
        cooldown = 500,                   -- Cooldown between clear attempts (ms)
        anim = {
            dict = 'anim@weapons@first_person@aim_rng@generic@pistol@singleshot@str',
            clip = 'reload_aim',
            duration = 750,
            blendIn = 2.0,
            blendOut = 2.0,
            flag = 51,
        },
    },
}
```

config\_whitelisted.lua

```lua
return {
    -- Jobs that are immune to gun jamming
    jobs = {
        'police',
        'sheriff',
        'ambulance',
        'mechanic',
    },

    -- Identifiers that are immune to gun jamming
    -- Supports: steam:, license:, discord:, fivem:, and ace groups
    identifiers = {
        -- 'steam:110000xxxxxxxxx',
        -- 'license:xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx',
        -- 'discord:123456789012345678',
        -- 'fivem:123456',
        -- 'group.admin',
        -- 'group.moderator',
    },
}
```

config\_weapons.lua

```lua
-- Weapon-specific jam chances (overrides default)
-- Use weapon hash names
-- Set to 0 to disable jamming for specific weapons
-- Set to -1 to use default chance
return {
    -- Pistols (15-20% - simpler mechanisms)
    [`WEAPON_PISTOL`] = 18.0,
    [`WEAPON_PISTOL_MK2`] = 15.0,
    [`WEAPON_COMBATPISTOL`] = 16.0,
    [`WEAPON_APPISTOL`] = 18.0,
    [`WEAPON_PISTOL50`] = 17.0,
    [`WEAPON_SNSPISTOL`] = 22.0,           -- Cheap gun, jams more
    [`WEAPON_SNSPISTOL_MK2`] = 18.0,
    [`WEAPON_HEAVYPISTOL`] = 16.0,
    [`WEAPON_VINTAGEPISTOL`] = 25.0,       -- Old gun, jams more
    [`WEAPON_MARKSMANPISTOL`] = 15.0,
    [`WEAPON_REVOLVER`] = 5.0,             -- Revolvers rarely jam
    [`WEAPON_REVOLVER_MK2`] = 4.0,
    [`WEAPON_DOUBLEACTION`] = 5.0,
    [`WEAPON_CERAMICPISTOL`] = 20.0,
    [`WEAPON_NAVYREVOLVER`] = 6.0,
    [`WEAPON_GADGETPISTOL`] = 17.0,

    -- SMGs (25-35% - high rate of fire)
    [`WEAPON_MICROSMG`] = 32.0,
    [`WEAPON_SMG`] = 28.0,
    [`WEAPON_SMG_MK2`] = 24.0,
    [`WEAPON_ASSAULTSMG`] = 30.0,
    [`WEAPON_COMBATPDW`] = 26.0,
    [`WEAPON_MACHINEPISTOL`] = 35.0,
    [`WEAPON_MINISMG`] = 33.0,
    [`WEAPON_RAYCARBINE`] = 0,             -- Alien weapon - no jam

    -- Rifles (20-30% - complex mechanisms)
    [`WEAPON_ASSAULTRIFLE`] = 25.0,
    [`WEAPON_ASSAULTRIFLE_MK2`] = 20.0,
    [`WEAPON_CARBINERIFLE`] = 24.0,
    [`WEAPON_CARBINERIFLE_MK2`] = 20.0,
    [`WEAPON_ADVANCEDRIFLE`] = 22.0,
    [`WEAPON_SPECIALCARBINE`] = 24.0,
    [`WEAPON_SPECIALCARBINE_MK2`] = 20.0,
    [`WEAPON_BULLPUPRIFLE`] = 26.0,
    [`WEAPON_BULLPUPRIFLE_MK2`] = 22.0,
    [`WEAPON_COMPACTRIFLE`] = 30.0,        -- Compact = more issues
    [`WEAPON_MILITARYRIFLE`] = 22.0,
    [`WEAPON_HEAVYRIFLE`] = 24.0,
    [`WEAPON_TACTICALRIFLE`] = 21.0,

    -- Machine Guns (35-45% - heat buildup, complexity)
    [`WEAPON_MG`] = 40.0,
    [`WEAPON_COMBATMG`] = 38.0,
    [`WEAPON_COMBATMG_MK2`] = 35.0,
    [`WEAPON_GUSENBERG`] = 45.0,           -- Old gun, jams more

    -- Shotguns (10-20% - simpler action)
    [`WEAPON_PUMPSHOTGUN`] = 12.0,
    [`WEAPON_PUMPSHOTGUN_MK2`] = 10.0,
    [`WEAPON_SAWNOFFSHOTGUN`] = 15.0,
    [`WEAPON_ASSAULTSHOTGUN`] = 18.0,
    [`WEAPON_BULLPUPSHOTGUN`] = 16.0,
    [`WEAPON_MUSKET`] = 0,                 -- Single shot - no jam
    [`WEAPON_HEAVYSHOTGUN`] = 20.0,
    [`WEAPON_DBSHOTGUN`] = 8.0,            -- Break action, very reliable
    [`WEAPON_AUTOSHOTGUN`] = 22.0,
    [`WEAPON_COMBATSHOTGUN`] = 17.0,

    -- Sniper Rifles (5-10% - precision weapons, well-maintained)
    [`WEAPON_SNIPERRIFLE`] = 8.0,
    [`WEAPON_HEAVYSNIPER`] = 7.0,
    [`WEAPON_HEAVYSNIPER_MK2`] = 5.0,
    [`WEAPON_MARKSMANRIFLE`] = 10.0,
    [`WEAPON_MARKSMANRIFLE_MK2`] = 8.0,
    [`WEAPON_PRECISIONRIFLE`] = 6.0,

    -- Special/Heavy (no jam)
    [`WEAPON_RPG`] = 0,
    [`WEAPON_GRENADELAUNCHER`] = 0,
    [`WEAPON_MINIGUN`] = 0,
    [`WEAPON_FIREWORK`] = 0,
    [`WEAPON_RAILGUN`] = 0,
    [`WEAPON_HOMINGLAUNCHER`] = 0,
    [`WEAPON_COMPACTLAUNCHER`] = 0,
    [`WEAPON_RAYMINIGUN`] = 0,

    -- Melee & Other (no jam)
    [`WEAPON_STUNGUN`] = 0,
    [`WEAPON_FLAREGUN`] = 0,
    [`WEAPON_PETROLCAN`] = 0,
    [`WEAPON_FIREEXTINGUISHER`] = 0,
}

```

config\_weapongroups.lua

```lua
-- Weapon group jam chances (fallback if specific weapon not listed)
-- Uses GTA weapon group hashes
return {
    [416676503] = 18.0,     -- GROUP_PISTOL
    [3337201093] = 30.0,    -- GROUP_SMG
    [970310034] = 25.0,     -- GROUP_ASSAULTRIFLE
    [1159398588] = 40.0,    -- GROUP_MG
    [-957766203] = 15.0,    -- GROUP_SHOTGUN
    [3082541095] = 8.0,     -- GROUP_SNIPER
    [-1212426201] = 0,      -- GROUP_HEAVY
    [1548507267] = 0,       -- GROUP_THROWN
    [-728555052] = 0,       -- GROUP_PETROLCAN
}

```
