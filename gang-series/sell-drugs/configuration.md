---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

```lua
return {
    debug = true,

    enableCommand = true,
    commandNames = { 'selldrugs', 'selldrug', 'trap' },
    commandHelp = 'Sell your current drugs for money',

    enableKeybind = false,
    keybindName = 'O',

    rejectChance = 30, -- % chance that an NPC will reject the transaction
    preventWeaponSelling = true, -- Prevents drug selling while holding a weapon

    police = {
        requiredAmount = 0,  -- Minimum number of online police required to sell
        jobs = { 'police' }, -- Jobs counted as police
        multiplier = {
            ['1'] = 1.0,
            ['2'] = 1.3,
            ['3'] = 1.4,
            ['4'] = 1.5
            --- ... and etc for any amount of cops
        }
    },
    ranks = {
        xpBasedOnAmount = true, -- If true, XP gained is based on drug amount sold
        levels = {
            --[[
                Rank Progression Settings

                Fields:
                - name        : (string) Internal rank identifier
                - label       : (string) Friendly name shown to players
                - color       : (string) Hex color for UI visuals
                - level       : (number) Numeric level in the rank progression
                - xpIncrease  : (number) XP added per transaction (unless overridden by xpBasedOnAmount)
                - rankXP      : (number) Required XP to reach this rank
                - multiplier  : (number) Earnings multiplier applied at this rank
            ]]
            { name = 'cornerboy', label = 'Cornerboy', color = '#FF0000', level = 1, xpIncrease = 1, rankXP = 2500, multiplier = 1.0 },
            { name = 'trapper', label = 'Trapper', color = '#00FF00', level = 2, xpIncrease = 1, rankXP = 5500, multiplier = 1.01 },
            { name = 'trapstar', label = 'Trapstar', color = '#0000FF', level = 3, xpIncrease = 1, rankXP = 14000, multiplier = 1.04 },
            { name = 'dopemover', label = 'Dope Mover', color = '#FF00FF', level = 4, xpIncrease = 1, rankXP = 20000, multiplier = 1.07 },
        }
    },
    drugs = {
        --[[
            Drug Configuration

            Fields:
            - price   : (number) Price per unit of the drug
            - max     : (number) Maximum quantity that can be sold in one transaction
            - account : (string) Account type where payment will be deposited (e.g., 'black_money')
        ]]
        ['coke'] = { price = 275, max = 10, account = 'black_money' },
    },
}
```
