---
description: >-
  Everything you need to get crate drops running on your server, including the
  supply signal item and the inventory stash it uses.
---

# Installation

## Dependencies

* A supported framework: ESX, QB or QBOX
* A supported inventory: ox\_inventory, qb-inventory or qs-inventory
* A supported target: ox\_target or qb-target
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)

{% hint style="info" %}
vanish\_gangs is optional. If it is running and gang integration is enabled in the config, gang members get notified when a crate drops.
{% endhint %}

## Steps

1. Download from keymaster and drag `vanish_cratedrops` into your `resources` folder.
2. Add `ensure vanish_cratedrops` to your `server.cfg`, after your framework, inventory and target resources.
3. Open `shared/config.lua` and set it up the way you want (drop frequency, timings, tiers and so on).
4. Set your loot tables in `shared/config_tiers.lua` to match the items on your server.
5. Start, or restart, the resource.

## Supply signal item

The supply signal lets a player call in a drop on themselves. It is optional. If you do not want players triggering drops, skip this section and rely on automatic and admin drops.

The item name is set by `supplySignal.itemName` in `shared/config.lua` (default `supply_signal`).

### ox\_inventory

Add the item to your `ox_inventory/data/items.lua`:

```lua
['supply_signal'] = {
    label = 'Supply Signal',
    weight = 500,
    stack = true,
    close = true,
    description = 'Pop this to call in an air drop on your position.',
    client = {
        export = 'vanish_cratedrops.useSupplySignal'
    }
},
```

### qb-inventory / qs-inventory

The signal export runs client side, so for QB and QS you register the usable item yourself and have it tell the client to fire the export. Add `supply_signal` to your shared items list, then drop this snippet into any of your own resources:

```lua
-- server side (your own resource)
QBCore.Functions.CreateUseableItem('supply_signal', function(source)
    TriggerClientEvent('myserver:useSupplySignal', source)
end)
```

```lua
-- client side (your own resource)
RegisterNetEvent('myserver:useSupplySignal', function()
    exports.vanish_cratedrops:useSupplySignal()
end)
```

{% hint style="warning" %}
Give players the item with the same name you set in `supplySignal.itemName`. If the names do not match, the signal will not validate and nothing will drop.
{% endhint %}

## Loot items

The default tiers ship with safe placeholder items (water, bread, bandage) so the script works out of the box. Before going live, open `shared/config_tiers.lua` and swap in real items from your server. Any item you list must exist in your inventory, otherwise it is skipped when the crate is filled.
