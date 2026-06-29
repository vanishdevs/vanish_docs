---
description: >-
  Setup for vanish_robbing, including the database table, framework bridge,
  target bridge and optional dispatch, gang and logging integrations.
---

# Installation

## Dependencies

* A supported framework: ESX, QB or QBOX
* ox\_inventory
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)
* ox\_target
* oxmysql

### Optional

* A dispatch system for police alerts: ps-dispatch, cd\_dispatch, qs-dispatch, rcore\_dispatch or linden\_outlawalert
* vanish\_gangs for gang-based robbery rules

{% hint style="info" %}
A bridge is included for each supported framework, dispatch system and logging adapter. If yours is not listed, the bridge files are short and easy to copy and adjust.
{% endhint %}

## Steps

1. Download from keymaster and drag `vanish_robbing` into your `resources` folder.
2. Import `sql/vanish_robbing.sql` into your database. This creates the `robbery_stats` table used by the leaderboard.
3. Add `ensure vanish_robbing` to your `server.cfg`, after your framework, ox\_lib, ox\_inventory, ox\_target and oxmysql.
4. Open `shared/config.lua` and set your input method, cooldowns, gang rules and notification settings.
5. Edit `shared/config_whitelistitems.lua` to protect items that should never be taken from a victim.
6. Edit `shared/config_blacklistplayers.lua` to set which jobs cannot rob or be robbed.
7. If you want logging, open `bridge/logging/config.lua` and enable your preferred adapter.
8. Start, or restart, the resource.

{% hint style="warning" %}
Item names in `config_whitelistitems.lua` must match the item names in your ox\_inventory exactly. A name that does not exist in the inventory is silently ignored.
{% endhint %}
