---
description: A quick install. Drop it in, set your items, and you are done.
---

# Installation

## Dependencies

* A supported framework: ESX, QB, QBOX or ox\_core
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)
* An inventory that holds the items you list (ox\_inventory or your framework's default)

## Steps

1. Download from keymaster and drag `vanish_pawnshop` into your `resources` folder.
2. Add `ensure vanish_pawnshop` to your `server.cfg`, after your framework and ox\_lib.
3. Open `shared/config.lua` and add the items you want players to buy and sell.
4. Set the ped location, or locations, where the shop should appear.
5. Start, or restart, the resource.

{% hint style="warning" %}
Every item you list under `BuyItems` or `SellItems` must exist in your inventory. Use the item's spawn name, not its display label.
{% endhint %}
