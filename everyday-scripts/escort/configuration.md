---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

```lua
return {
    -- Feature Toggles
    useTarget = true,                   -- Enable ox_target support
    useKeybind = true,                  -- Enable keybind usage for escorting
    usageKeybind = 'E',                 -- Default keybind to start escorting (keyboard key)
    useCommand = true,                  -- Enable command usage (e.g., /escort)

    -- Permissions
    restrictedJobs = false,            -- Restrict usage to specific jobs (e.g., { 'police', 'ambulance' })
    restrictedCommandGroups = { 
        'group.admin', 
        'group.user' 
    },                                 -- Restrict command to specific ACE groups or identifiers as supported; set to false to disable

    -- Escort Settings
    releaseKeybind = 38,               -- Key to release escorted player (default: E)
    escortDistance = 3.0,              -- Max distance to initiate escort (recommend keeping low)
    escortDeadPlayers = true,          -- Allow escorting dead players
    forceUnarmed = true,               -- Strip escorting player of weapons during escort
    disableControls = true,            -- Disable controls while escorting or being escorted

    -- Disabled controls
    disabledControls = {
        escorting = {
            22, 24, 25, 37, 44, 45, 68, 69, 91, 92, 140, 141, 142, 143, 257, 263, 264
        },
        escorted = {
            22, 23, 24, 25, 37, 44, 45, 68, 69, 75,
            140, 141, 142, 143, 167, 170, 288, 289
        }
    },  

    -- Animations
    anims = {
        escorting = {
            onFoot = {dict = 'amb@code_human_wander_drinking_fat@beer@male@base', anim = 'static'}
        },
        escorted = {
            walk = {dict = 'move_m@brave@a', anim = 'walk'},
            run = {dict = 'move_m@quick', anim = 'walk'},
        }
    }
}
```
