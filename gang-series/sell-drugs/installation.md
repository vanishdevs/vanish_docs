---
description: >-
  Setup for selling drugs, including the database table, the inventory it reads
  from, and the optional dispatch and ambulance bridges.
---

# Installation

## Dependencies

* A supported framework: ESX, QB or QBOX
* ox\_inventory
* oxmysql
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)

### Optional

* A target system if you want NPC target interaction: ox\_target or qb-target
* A dispatch system for police alerts: cd\_dispatch, linden\_outlawalert, ps-dispatch, qs-dispatch or rcore\_dispatch
* An ambulance system: esx\_ambulancejob or wasabi\_ambulance

{% hint style="info" %}
A bridge is included for each supported framework, target, dispatch and ambulance system. If yours is not listed, the bridge files are easy to copy and adjust.
{% endhint %}

## Steps

1. Download from keymaster and drag `vanish_selldrugs` into your `resources` folder.
2. Import `sql/vanish_selldrugs.sql` into your database. This creates the `drug_ranks` table used for XP, ranks and the leaderboard.
3. Add `ensure vanish_selldrugs` to your `server.cfg`, after your framework, ox\_inventory and oxmysql.
4. Add your drug items to `shared/config_drugs.lua` (they must already exist in your inventory).
5. Set your sell zones in `shared/config_zones.lua`.
6. Adjust ranks, police scaling and the rest to taste.
7. Start, or restart, the resource.

{% hint style="warning" %}
The drug item names in `config_drugs.lua` must match the item names in your inventory exactly. An item that does not exist in your inventory cannot be sold.
{% endhint %}
