---
description: >-
  How drops are created, how players call them in, and how to tune tiered loot
  without breaking the flow.
---

# Drop Lifecycle and Tuning

Crate drops can start automatically, from admin commands, or from a player using
the supply signal item. All three paths end in the same public event: a plane
flies over, drops a crate, and players fight over the unlock window.

## Drop Lifecycle

1. A drop is requested by the timer, `/createdrop`, `/createdrophere`, an export, or a supply signal.
2. The script checks player count, active drop limit and coordinate rules unless the drop is forced.
3. A location and tier are selected.
4. The crate blip appears and the plane arrives after `timings.planeDelay`.
5. The plane releases the crate near the drop point.
6. The flare and crate effects mark the landing area.
7. Players wait through `timings.unlockDelay` before the crate can be opened.
8. The crate inventory opens and players loot the generated items.
9. Empty crates clean up after `timings.cleanupDelay`; untouched crates expire after `timings.totalDuration`.

That sequence is controlled mostly by the `timings`, `plane`, `crate` and
`zones` blocks in `shared/config.lua`.

{% hint style="info" %}
The unlock delay is what creates the fight. If crates are being looted with no
contest, increase `unlockDelay` or make the drop announcement more visible.
{% endhint %}

## Supply Signals

The supply signal lets a player call a crate near their own position. The item
name comes from `supplySignal.itemName`, default `supply_signal`.

When a player uses it:

1. The client starts the placement animation.
2. The server validates the item if `supplySignal.validateItem = true`.
3. The player is checked against `supplySignal.cooldown`.
4. The requested coordinates are checked against `coordinateValidation`.
5. A crate drop is created at or near the placement point.

For ox_inventory, wire the item to the client export shown on the installation
page. For QB and QS inventory, register a usable item in one of your own
resources and trigger the same client export.

{% hint style="warning" %}
The inventory item name and `supplySignal.itemName` must match exactly. If they
do not match, the signal will validate as missing and no crate will be called.
{% endhint %}

## Tuning Tiers

Each tier in `shared/config_tiers.lua` controls:

| Field | What it changes |
| --- | --- |
| `spawnChance` | How often automatic drops roll that tier. |
| `minItems` / `maxItems` | How many loot entries the crate tries to include. |
| `blipColor`, `blipSprite`, `crateModel`, `particleColor` | How the tier looks. |
| `skillcheck` | Whether opening the crate requires an ox_lib skillcheck. |
| `lootItems` | The item pool, amount range and chance for each item. |

Start conservative. Make common crates useful but not economy-breaking, then use
rare tiers for items that should create server-wide attention.

A good production pass is:

1. Replace the default food and water items with your real loot.
2. Confirm every item exists in your active inventory.
3. Keep common tier chance high enough that automatic drops do not feel empty.
4. Test a forced legendary drop with `/createdrophere legendary` before opening it to players.
5. Watch webhook logs for opened, looted and expired crates.

## Automatic vs Admin Drops

Automatic drops use `automaticDrops.intervalMinutes`, `minimumPlayers`,
`maxConcurrentDrops` and the tier spawn chances. Admin drops are better for
events because staff can force a tier and location.

Use `/createdrop` for a random configured location. Use `/createdrophere` when
you want the fight at your current position.
