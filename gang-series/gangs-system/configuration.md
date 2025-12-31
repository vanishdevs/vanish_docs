---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

#### config.lua

```lua
return {
    -- Enable debug mode for detailed logging
    debug = true,

    -- Gang Administration
    admin = {
        -- Keybind to open gang admin menu
        keybind = 'F9',

        -- Permission groups for gang administration
        -- Set to false to allow all players to use the command
        permissions = {
            migrateranks = { 'group.god', 'group.admin' },
            gangadmin = { 'group.god', 'group.admin', 'group.mod' },

            creategang = { 'group.god', 'group.admin', 'group.mod' },
            createrank = { 'group.god', 'group.admin', 'group.mod' },
            deletegang = { 'group.god', 'group.admin', 'group.mod' },
            deleterank = { 'group.god', 'group.admin', 'group.mod' },

            setpasscode = { 'group.god', 'group.admin', 'group.mod' },
            setgang = { 'group.god', 'group.admin', 'group.mod' },
            removefromgang = { 'group.god', 'group.admin', 'group.mod' },

            -- Player commands (set to false to allow all players)
            currentgang = false,
            acceptinvite = false,
            declineinvite = false,
            leavegang = false
        }
    },

    -- Gang Information Display (HUD)
    hud = {
        -- Enable gang info display
        enabled = true,

        -- Only show when player is in a gang
        showOnlyInGang = true,

        -- Default values for players not in a gang (only used when showOnlyInGang = false)
        defaults = {
            gangName = 'Unwanted',
            rankName = 'Bum'
        },

        -- Visual settings
        position = {
            bottom = '10px',
            right = '20px',
            top = 'auto',
            left = 'auto'
        },
        backgroundColor = 'rgba(51, 51, 51, 0.0)',
        boxShadow = '0 0 15px rgba(0, 0, 0, 0.0)'
    },

    -- Gang Management System
    management = {
        -- Location mode: 'single' for one shared location, 'multiple' for gang-specific locations
        -- For multiple locations, configure in shared/config_managements.lua
        mode = 'single',

        -- Single location settings (only used when mode = 'single')
        location = {
            coords = vector3(-268.4806, -957.1339, 31.2231),
            heading = 208.3381
        },

        -- Interaction type: 'ped' for NPC interaction, 'marker' for ground marker
        interactionType = 'ped',

        -- Interaction distance in game units
        interactionDistance = 10.0,

        -- NPC (Ped) configuration
        npc = {
            enabled = true,
            model = 'g_m_importexport_01',
            scenario = 'WORLD_HUMAN_CLIPBOARD'
        },

        -- Ground marker configuration
        marker = {
            enabled = false,
            type = 1,
            size = vec3(1.0, 1.0, 0.1),
            color = vec3(255, 255, 255)
        },

        -- Chat notification settings
        notifications = {
            enabled = true,
            template = '<div style="padding: 0.5vw; margin: 0.5vw; background-color: rgba(255, 0, 0, 0.6); border-radius: 3px;">💀 {0}</div>'
        }
    },

    -- Gang Inventory Settings
    inventory = {
        slots = 50,
        weight = 100000
    }
}

```

#### config\_managements.lua

```lua
--[[
    Gang-Specific Management Locations

    Only edit this file when management.mode = 'multiple' in shared/config.lua

    Each location is dedicated to a specific gang. Only leaders of that gang
    can interact with their designated location.

    Configuration:
    - gang: The internal gang name (must match database gang names)
    - coords: The world coordinates for the management point
    - heading: The direction the NPC will face (if using NPC interaction)
]]

return {
    {
        gang = 'ballas',
        coords = vector3(-232.1606, -974.2057, 29.2886),
        heading = 287.1345
    },
    -- {
    --     gang = 'vagos',
    --     coords = vector3(-269.0714, -955.8455, 31.2231),
    --     heading = 90.0
    -- },
    -- {
    --     gang = 'families',
    --     coords = vector3(-30.0, -1434.0, 31.0),
    --     heading = 270.0
    -- },
}
```
