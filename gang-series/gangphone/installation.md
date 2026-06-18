---
description: >-
  Setup for the gang phone, covering the database, the gang system it reads from,
  and the phone item.
---

# Installation

## Dependencies

* A supported framework: ESX, QB or QBOX
* A supported gang system: vanish\_gangs, qb-gangs or rcore\_gangs
* A supported inventory: ox\_inventory, qb-inventory or qs-inventory
* oxmysql
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)
* OneSync enabled, server artifact build 5848 or newer

{% hint style="info" %}
The phone reads who is in which gang from your gang system, so you need one of the supported ones running. Ambulance and crutch scripts are optional and only used to make kill detection more accurate.
{% endhint %}

## Steps

1. Download from keymaster and drag `vanish_gangphone` into your `resources` folder.
2. Import `sql/vanish_gangphone.sql` into your database. This creates the war, challenge, stats and leaderboard tables.
3. Add `ensure vanish_gangphone` to your `server.cfg`, after your framework, gang system, inventory and oxmysql.
4. Add the phone item to your inventory (see below).
5. Open `shared/config.lua` and set it up the way you want.
6. Start, or restart, the resource.

## Phone item

By default the phone needs an inventory item to open, set by `phone.itemName` in `shared/config.lua` (default `burnerphone`). Add that item to your inventory.

### ox\_inventory

Add the item to your `ox_inventory/data/items.lua`:

```lua
['burnerphone'] = {
    label = 'Burner Phone',
    weight = 200,
    stack = false,
    close = true,
    description = 'A throwaway phone for gang business.',
    client = {
        export = 'vanish_gangphone.OpenPhone'
    }
},
```

For qb-inventory and qs-inventory, add `burnerphone` to your shared items, then register it as a usable item that calls the `OpenPhone` export on the client. See the [Exports](exports.md) page for the export.

{% hint style="info" %}
You do not have to use an item. Set `phone.enabled = false` to let players open the phone with the command or keybind alone, or set `phone.command` / `ui.openKey` to your liking.
{% endhint %}

{% hint style="warning" %}
If `phone.leaderOnly = true`, only gang leaders can open the phone at all. Leave it false to let any gang member open it, while still gating war actions behind the rank permissions.
{% endhint %}
