---
description: >-
  Setup for Shops, including framework bridges, ox_inventory item handling,
  config files and shop commands.
---

# Installation

## Dependencies

* A supported framework: ESX, QB or QBOX
* ox_inventory
* ox_lib [https://github.com/overextended/ox_lib](https://github.com/overextended/ox_lib)

### Optional

* ox_target or qb-target if you want target interaction instead of markers.
* oxmysql if your packaged build expects the manifest import to be present.

## Steps

1. Download from keymaster and drag `vanish_shops` into your `resources` folder.
2. Add `ensure vanish_shops` to your `server.cfg`, after your framework, ox_inventory and ox_lib.
3. Configure the global command, interaction defaults, rotation and image path in `shared/config.lua`.
4. Set payment methods in `shared/config_paymentmethods.lua`.
5. Set shop modes in `shared/config_modes.lua`.
6. Place shops in `shared/config_locations.lua`.
7. Add items, stock and prices in `shared/config_items.lua`.
8. Start, or restart, the resource.

{% hint style="warning" %}
Every item in `shared/config_items.lua`, and every item-based payment in
`shared/config_paymentmethods.lua`, must exist in ox_inventory.
{% endhint %}
