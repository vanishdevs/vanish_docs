---
description: >-
  Setup for the money wash resource, including framework requirements and the
  dirty-money account it converts.
---

# Installation

## Dependencies

* A supported framework: ESX, QB or QBOX
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)

## Steps

1. Download from keymaster and drag `vanish_moneywash` into your `resources` folder.
2. Add `ensure vanish_moneywash` to your `server.cfg`, after your framework and ox\_lib.
3. Open `shared/config.lua` and set your wash locations, fees and limits.
4. Start, or restart, the resource.

{% hint style="info" %}
The script washes the `black_money` account into clean cash. If your server uses a different name for dirty money, adjust it to match your framework setup.
{% endhint %}
