---
description: >-
  The export for starting a sale from your own code, plus the commands the
  script registers.
---

# Exports

## Server

### startDrugSale

Starts a sale for a player from your own resource. This runs the same flow as the command and keybind, so the zone, picker, police and rank rules all still apply.

```lua
local started = exports.vanish_selldrugs:startDrugSale(source, drug)
```

| Argument | Type | Notes |
| --- | --- | --- |
| `source` | number | The player's server id. |
| `drug` | string / nil | An item name to sell. Leave it out to let the picker decide based on the config. |

Returns: `true` if the sale flow started, `false` if it could not (for example no source given).

```lua
-- Let the picker choose what to sell
exports.vanish_selldrugs:startDrugSale(source)

-- Force a coke sale
exports.vanish_selldrugs:startDrugSale(source, 'coke')
```

## Commands

| Command | Description |
| --- | --- |
| `/selldrugs`, `/selldrug`, `/trap` | Start a sale. The aliases are set by `command.aliases` in `shared/config.lua`. |
| `/drugleaderboard` | Open the dealer leaderboard. The name is set by `leaderboard.commandName`. |

Both commands can be turned off in the config (`command.enabled` and `leaderboard.enableCommand`). Players can also start a sale with the optional keybind if `keybind.enabled` is true.
