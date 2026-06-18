---
description: >-
  The full config with each block explained. Setting up your buy and sell lists
  is the main thing you will do here.
---

# Configuration

## shared/config.lua

```lua
return {
    -- Opening Hours, if applicable
    OpeningHours = {
        enable = true,
        open = { hour = 21, minute = 0 },
        close = { hour = 23, minute = 0 },
    },

    -- Blip Settings
    Blip = {
        enabled = true,
        label = 'Pawnshop',
        sprite = 605,
        colour = 5,
        scale = 0.8,
    },

    -- Ped Settings
    Ped = {
        model = 'g_m_m_armgoon_01',
        scenario = 'WORLD_HUMAN_STAND_IMPATIENT',
        locations = {
            { coords = vector3(182.4168, -1319.2084, 28.3163), heading = 236.6841 },
        },
    },

    -- Items players can buy from the shop
    BuyItems = {
        enable = true,
        accountType = 'money',
        items = {
            -- { name = 'weapon_bat', label = 'Baseball Bat', icon = 'fas fa-baseball-bat-ball', price = 1000 },
        },
    },

    -- Items players can sell to the shop
    SellItems = {
        enable = true,
        accountType = 'money',
        items = {
            -- { name = 'weapon_bat', label = 'Baseball Bat', icon = 'fas fa-baseball-bat-ball', price = 1000 },
        },
    },
}
```

## Opening hours

When `OpeningHours.enable` is true, the shop only works between the `open` and `close` times, set on the 24 hour clock. Set `enable` to false to leave it open around the clock.

## Ped and locations

`Ped.model` and `Ped.scenario` set how the shop clerk looks and behaves. Add an entry to `locations` for every place you want a pawnshop to appear. Each entry takes `coords` and a `heading` for the ped to face.

## Buy and sell lists

`BuyItems` and `SellItems` are separate lists, and each has its own `enable` toggle, so you can run a buy-only or sell-only shop if you want.

Each item takes four values:

| Field | What it is |
| --- | --- |
| `name` | The item's spawn name, the same one your inventory uses. |
| `label` | The name shown to players in the menu. |
| `icon` | A Font Awesome v6 icon class, for example `fas fa-baseball-bat-ball`. |
| `price` | The value **in cents**. `1000` is $10.00, `100` is $1.00. |

{% hint style="warning" %}
Prices are in cents, not dollars. If you want an item to sell for $50, set `price = 5000`.
{% endhint %}

`accountType` sets which balance is used for the transaction (for example `money`, `bank` or `black_money`, depending on your framework).
