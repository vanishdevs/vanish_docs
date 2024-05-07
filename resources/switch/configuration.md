---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

```lua
return {
    SwitchItem = 'switch',
    Weapons = {
        -- Original Weapon => Switch Weapon (Both Items must exist in the server)
        ['WEAPON_GLOCK19']  = 'WEAPON_GLOCK19S',
        ['WEAPON_G26']  = 'WEAPON_GLOCK26S',
        ['WEAPON_G17']  = 'WEAPON_GLOCK17S',
    }
}

```
