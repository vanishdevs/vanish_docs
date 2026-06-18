---
description: >-
  Setup for the gang system, including the database, the stash inventory and the
  optional target system. This is the base the rest of the gang series builds on.
---

# Installation

## Dependencies

* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)
* oxmysql
* ox\_inventory (for the shared gang stash)

### Optional

* A framework, if you want gang membership tied to it: ESX, QB or QBOX. The system also runs fully standalone.
* A target system for management points: ox\_target or qb-target

{% hint style="info" %}
This is the core resource for the gang series. If you plan to run Turfs, Gang Phone or Crate Drops with gang features, install this first.
{% endhint %}

## Steps

1. Download from keymaster and drag `vanish_gangs` into your `resources` folder.
2. Import `sql/vanish_gangs.sql` into your database. This creates the tables for gangs, ranks, members and the rest.
3. Add `ensure vanish_gangs` to your `server.cfg`, after ox\_lib, oxmysql and ox\_inventory (and your framework, if you use one).
4. Open `shared/config.lua` and set your admin permissions, management points and HUD.
5. Start, or restart, the resource.

## First steps after install

1. Make sure your admin account is in one of the groups listed under `admin.permissions` in `shared/config.lua`.
2. In-game, open the admin menu with `/gangadmin` (or the keybind) and create your first gang. The [Gang Administration](guides/gang-administration.md) guide walks through this.
3. Set the gang a passcode so leaders can open its stash.
4. Invite a player and promote them to leader so they can manage the gang from a management point. See [Gang Management](guides/gang-management.md).

{% hint style="warning" %}
The gang stash and its per-rank withdrawal limits run through ox\_inventory. If you do not run ox\_inventory, the stash features will not work.
{% endhint %}
