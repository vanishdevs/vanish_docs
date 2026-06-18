---
description: >-
  Setup for the turf system, including the database, the gang system it reads
  from, and the optional target and ambulance bridges.
---

# Installation

## Dependencies

* A supported framework: ESX, QB or QBOX
* A supported gang system: vanish\_gangs or rcore\_gangs
* ox\_inventory (used for stash rewards and loot dropoffs)
* oxmysql
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)

### Optional

* ox\_target, if you want to open loot dropoffs with the third-eye instead of a key
* An ambulance system for respawn handling: esx\_ambulancejob, qb-ambulancejob or wasabi\_ambulance

{% hint style="info" %}
Turfs reads which gang a player belongs to from your gang system, so one of the supported ones needs to be running.
{% endhint %}

## Steps

1. Download from keymaster and drag `vanish_turfs` into your `resources` folder.
2. Import `sql/vanish_turfs.sql` into your database. This creates the tables for stats, the leaderboard and per-player settings.
3. Add `ensure vanish_turfs` to your `server.cfg`, after your framework, gang system, ox\_inventory and oxmysql.
4. Open `shared/config.lua` and set your reward mode, cooldowns and rules.
5. Build your zones, either in `shared/config.lua` or live with the in-game editor (see below).
6. Start, or restart, the resource.

## Building zones with the editor

You do not have to write zones by hand. Run `/turfeditor` in-game (admin only by default) to place start and capture points, set capture time and rewards, and save, all without touching the config or restarting. The editor caps are set under the `editor` block in the config.
