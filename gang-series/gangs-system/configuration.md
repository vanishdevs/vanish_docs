---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

```lua
return {
    Debug = true,
    
    -- These groups will have access to create gangs, delete them and etc
    GangAdminKeybind = 'F9',
    AdminGroups = {
        gangadmin = { 'group.god', 'group.admin', 'group.mod' },
        
        creategang = { 'group.god', 'group.admin', 'group.mod' },
        createrank = { 'group.god', 'group.admin', 'group.mod' },
        deletegang = { 'group.god', 'group.admin', 'group.mod' },
        deleterank = { 'group.god', 'group.admin', 'group.mod' },

        setpasscode = { 'group.god', 'group.admin', 'group.mod' },
        setgang = { 'group.god', 'group.admin', 'group.mod' },
        removefromgang = { 'group.god', 'group.admin', 'group.mod' },
        
        currentgang = false,
        acceptinvite = false,
        declineinvite = false,
        leavegang = false
    },

    DisplayGangInfo = true, -- Display a small UI at the bottom displaying the information of the gang you are in
    OnlyDisplayInGang = true, -- Only display the UI if you are in a gang
    GangInfoSettings = {
        backgroundColor = 'rgba(51, 51, 51, 1.0)',
        boxShadow = '0 0 15px rgba(0, 0, 0, 0.5)',
    },

    Management = {
        LocationType = 'single',
        SingleLocationCoords = vector3(-268.4806, -957.1339, 31.2231),
        SingleLocationHeading = 258.8334,

        ChatAlerts = {
            enabled = true,
            template = '<div style="padding: 0.5vw; margin: 0.5vw; background-color: rgba(255, 0, 0, 0.6); border-radius: 3px;">💀 {0}</div>'
        },

        UseMarker = false,
        MarkerSettings = {
            distance = 10,
            type = 1,
            size = vec3(1, 1, 0.1),
            color = vec3(255, 255, 255)
        },

        UsePeds = true,
        PedSettings = {
            distance = 10,
            model = 'g_m_importexport_01',
            scenario = 'WORLD_HUMAN_CLIPBOARD'
        }
    }
}
```
