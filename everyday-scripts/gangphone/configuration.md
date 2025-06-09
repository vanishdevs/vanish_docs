---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

```lua
Config = {}

Config.GangPhoneItem = 'burnerphone'
Config.KillsToWin = 10

Config.Notify = function(msg, msgType, time)
    TriggerEvent('esx:showNotification', msg)
end
```
