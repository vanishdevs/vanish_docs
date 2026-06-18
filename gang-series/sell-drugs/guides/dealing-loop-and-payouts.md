---
description: >-
  How a sale starts, how single and autosell modes differ, and how zones,
  police, ranks and hot zones affect payout.
---

# Dealing Loop and Payouts

Sell Drugs is built around a short NPC sale loop. The config decides how the
player starts that loop, how the NPC interaction works, and what modifies the
final payout.

## Sale Flow

1. The player runs `/selldrugs`, `/selldrug`, `/trap`, presses the optional keybind, or another resource calls the export.
2. The script chooses a drug by `picker.mode`.
3. The player must be inside one of the zones in `shared/config_zones.lua`.
4. An NPC buyer is selected from `shared/config_peds.lua`.
5. The buyer walks to the player.
6. The player confirms the sale with the configured key or target option.
7. The sale may be accepted or rejected based on `sale.rejectChance`.
8. Police alerts can fire based on the outcome and `shared/config_police.lua`.
9. On success, the player gets paid and receives rank XP.

{% hint style="warning" %}
Players cannot sell items that do not exist in both their inventory and
`shared/config_drugs.lua`. Item names are case-sensitive.
{% endhint %}

## Single vs Autosell

`command.mode` changes how repeated selling feels.

| Mode | Player experience |
| --- | --- |
| `single` | Each command or keybind press attempts one sale. |
| `autosell` | First press starts repeated sales. Running the command again stops it. |

Autosell stops when the player runs out of the selected drug, leaves the zone,
dies, enters a vehicle, gets too far from the buyer, or toggles it off.

Use `single` for a slower, more deliberate economy. Use `autosell` if you want
selling to feel like a grind loop with less repeated input.

## Picker Mode

`picker.mode = 'auto'` sells the first configured drug the player has, following
the order in `shared/config_drugs.lua`.

`picker.mode = 'pick'` opens a menu so the player can choose from the drugs they
carry. Use pick mode when several drugs are common on your server and prices are
meaningfully different.

## Payout Formula

The final payout is shaped by several config files:

| Source | What it changes |
| --- | --- |
| `shared/config_drugs.lua` | Base price per unit and max units per transaction. |
| `shared/config_ranks.lua` | Rank multiplier and XP progression. |
| `shared/config_police.lua` | Minimum police count and police-count multiplier. |
| `shared/config_zones.lua` | Hot zone multiplier and flat bonus. |

The practical setup order is:

1. Set base drug prices first.
2. Add police multipliers so riskier hours pay more.
3. Add rank multipliers carefully, since long-term dealers will hit them often.
4. Use hot zones as temporary or location-based boosts.

## Hot Zones

Any sell zone can become a hot zone by adding a `hot` table:

```lua
hot = { multiplier = 1.5, bonus = 250 }
```

The multiplier applies after rank and police multipliers. The bonus is added on
top after multipliers.

Use hot zones to push players into contested parts of the map. Keep the radius
large enough for NPC pathing and small enough that players understand where the
boost applies.
