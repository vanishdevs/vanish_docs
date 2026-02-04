---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

config.lua

```lua
return {
    debug = false,

    -- Admin permissions for commands
    admin = {
        restock = { 'group.god', 'group.admin' },
        setstock = { 'group.god', 'group.admin' },
        rotate = { 'group.god', 'group.admin' },
    },

    -- Command to open the shop, nearest one (set to false to disable)
    command = 'shop',

    -- Keybind to open shop, nearest one (set to false to disable)
    keybind = false,
    keybindDescription = 'Open Shop',

    -- Global defaults for all locations (can be overridden per-location)
    defaults = {
        type = 'marker',           -- 'npc' or 'marker'
        spawnDistance = 50.0,   -- Distance to spawn NPC / activate marker

        npc = {
            model = `s_m_y_dealer_01`,
            scenario = 'WORLD_HUMAN_SMOKING',
        },

        marker = {
            type = 1,
            scale = { x = 1.0, y = 1.0, z = 0.5 },
            color = { r = 255, g = 255, b = 255, a = 100 },
        },

        target = {
            distance = 2.5,
        },
    },

    -- Enable rotating item system (items have a chance to appear)
    -- Items rotate on resource restart or manual command
    rotation = {
        enabled = true,            -- Set to true to enable rotating items
        maxItems = 15,              -- Maximum number of items to show at once (0 = no limit)
        guaranteedCategories = {},  -- Categories that always appear (e.g., {'ammo', 'tools'})
        minPerCategory = 1,         -- Minimum items per category (if category has items)
    },

    -- Configure where item images are loaded from
    images = {
        -- Base path for item images (NUI path format)
        -- Examples:
        --   'nui://ox_inventory/web/images/'
        --   'nui://qb-inventory/html/images/'
        --   'nui://qs-inventory/web/images/'
        path = 'nui://ox_inventory/web/images/',
    },

}

```

config\_paymentmethods.lua

```lua
return {
    -- type: 'money' or 'item'
    -- modes: (optional) Array of mode IDs where this payment is available
    --        If omitted or empty, payment is available in ALL modes
    --
    -- For item-based payments, specify the item name in the 'item' field
    --
    cash = {
        enabled = true,
        label = 'Cash',
        icon = 'cash',
        color = '#22c55e',
        type = 'money',
        -- modes = {}, -- Available in all modes
    },

    bank = {
        enabled = true,
        label = 'Bank',
        icon = 'bank',
        color = '#3b82f6',
        type = 'money',
        modes = { 'shop' },
    },

    black_money = {
        enabled = true,
        label = 'Dirty Money',
        icon = 'black_money',
        color = '#a855f7',
        type = 'money',
        modes = { 'blackmarket' },
    },

    diamonds = {
        enabled = true,
        label = 'Diamonds',
        icon = 'diamond',
        color = '#2527a1',
        type = 'item',
        item = 'diamonds',
        modes = { 'blackmarket' },
    },

    -- Example: Item-based payment
    -- gold_bar = {
    --     enabled = true,
    --     label = 'Gold Bars',
    --     icon = 'package',
    --     color = '#fbbf24',
    --     type = 'item',
    --     item = 'gold_bar',
    --     modes = { 'pawnshop' },
    -- },

    -- Example: Scrap metal for pawnshop
    -- scrap_metal = {
    --     enabled = true,
    --     label = 'Scrap Metal',
    --     icon = 'package',
    --     color = '#6b7280',
    --     type = 'item',
    --     item = 'scrap_metal',
    --     modes = { 'pawnshop' },
    -- },
}

```

config\_modes.lua

```lua
-- Each mode defines the shop's appearance, target icon, and blip settings
-- Locations using this mode will inherit these settings
return {
    shop = {
        title = 'General Store',
        subtitle = 'Quality Goods',
        theme = {
            primary = '#0a84ff',
            secondary = '#64d2ff',
            accent = '#0a84ff',
            icon = 'store',
            gradient = 'linear-gradient(135deg, #0a84ff 0%, #64d2ff 100%)',
        },
        categories = {
            { id = 'misc', label = 'General', icon = 'general', description = 'Everyday essentials' },
            { id = 'tools', label = 'Tools', icon = 'tools', description = 'Useful equipment' },
        },
        target = { icon = 'fas fa-store' },
        blip = { sprite = 52, color = 2, scale = 0.8 },
    },

    blackmarket = {
        title = 'Black Market',
        subtitle = 'Underground Trading',
        theme = {
            primary = '#ff453a',
            secondary = '#ff6b6b',
            accent = '#ff453a',
            icon = 'skull',
            gradient = 'linear-gradient(135deg, #ff453a 0%, #ff6b6b 100%)',
        },
        categories = {
            { id = 'weapons', label = 'Weapons', icon = 'gun', description = 'Firearms and melee weapons' },
            { id = 'ammo', label = 'Ammunition', icon = 'ammo', description = 'Bullets and magazines' },
            { id = 'attachments', label = 'Attachments', icon = 'attachment', description = 'Weapon modifications' },
            { id = 'armor', label = 'Protection', icon = 'armor', description = 'Body armor and protection' },
            { id = 'tools', label = 'Tools', icon = 'tools', description = 'Lockpicks, drills, and more' },
            { id = 'drugs', label = 'Substances', icon = 'drugs', description = 'Various substances' },
            { id = 'misc', label = 'Miscellaneous', icon = 'misc', description = 'Other items' },
            { id = 'medical', label = 'Medical', icon = 'medical', description = 'Healing and medical supplies' },
            { id = 'food', label = 'Food', icon = 'food', description = 'Consumable food items' },
            { id = 'drinks', label = 'Drinks', icon = 'drinks', description = 'Beverages and hydration' },
            { id = 'clothing', label = 'Clothing', icon = 'clothing', description = 'Apparel and accessories' },
            { id = 'jewelry', label = 'Jewelry', icon = 'jewelry', description = 'Luxury valuables and collectibles' },
            { id = 'pills', label = 'Pills', icon = 'pills', description = 'Prescription and experimental pills' },
            { id = 'vehicles', label = 'Vehicles', icon = 'vehicles', description = 'Vehicle-related items and parts' },
            { id = 'general', label = 'General', icon = 'general', description = 'General purpose items' },
        },
        target = { icon = 'fas fa-skull' },
        blip = { sprite = 110, color = 1, scale = 0.8 },
    },

    -- gunshop = {
    --     title = 'Ammu-Nation',
    --     subtitle = 'Tools for Self-Defense',
    --     theme = {
    --         primary = '#22c55e',
    --         secondary = '#4ade80',
    --         accent = '#22c55e',
    --         icon = 'gun',
    --         gradient = 'linear-gradient(135deg, #22c55e 0%, #4ade80 100%)',
    --     },
    --     categories = {
    --         { id = 'weapons', label = 'Weapons', icon = 'gun', description = 'Legal firearms' },
    --         { id = 'ammo', label = 'Ammunition', icon = 'ammo', description = 'Bullets and magazines' },
    --         { id = 'attachments', label = 'Attachments', icon = 'attachment', description = 'Weapon modifications' },
    --         { id = 'armor', label = 'Protection', icon = 'armor', description = 'Body armor and protection' },
    --     },
    --     target = { icon = 'fas fa-gun' },
    --     blip = { sprite = 110, color = 2, scale = 0.8 },
    -- },

    -- pharmacy = {
    --     title = 'Pharmacy',
    --     subtitle = 'Medical Supplies',
    --     theme = {
    --         primary = '#06b6d4',
    --         secondary = '#22d3ee',
    --         accent = '#06b6d4',
    --         icon = 'medical',
    --         gradient = 'linear-gradient(135deg, #06b6d4 0%, #22d3ee 100%)',
    --     },
    --     categories = {
    --         { id = 'misc', label = 'Medical', icon = 'medical', description = 'First aid and medication' },
    --     },
    --     target = { icon = 'fas fa-pills' },
    --     blip = { sprite = 51, color = 11, scale = 0.8 },
    -- },

    -- pawnshop = {
    --     title = 'Pawn Shop',
    --     subtitle = 'Buy, Sell, Trade',
    --     theme = {
    --         primary = '#f59e0b',
    --         secondary = '#fbbf24',
    --         accent = '#f59e0b',
    --         icon = 'jewelry',
    --         gradient = 'linear-gradient(135deg, #f59e0b 0%, #fbbf24 100%)',
    --     },
    --     categories = {
    --         { id = 'misc', label = 'Goods', icon = 'general', description = 'Second-hand items' },
    --     },
    --     target = { icon = 'fas fa-gem' },
    --     blip = { sprite = 605, color = 46, scale = 0.8 },
    -- },
}

```

&#x20;config\_locations.lua

```lua
-- Simple format: { id, mode, coords }
-- Blip label defaults to mode title, other settings come from config_modes.lua
-- All other settings (type, npc, marker, etc.) come from config.defaults
--
-- Optional overrides per location:
--   label = 'Custom Blip Name'  -- Override blip label
--   type = 'marker'             -- Override interaction type
--   blip = false                -- Disable blip for this location
--   enabled = false             -- Disable this location

return {
    -- Unique Black Market location
    { id = 'blackmarket_unique', mode = 'blackmarket', coords = vector4(-559.1788, -1803.9039, 22.6088, 153.0243) },

    -- General shops
    { id = 'shop_247_01', mode = 'shop', coords = vector4(373.8, 325.8, 103.5, 0.0) },
    { id = 'shop_247_02', mode = 'shop', coords = vector4(2557.4, 382.2, 108.6, 0.0) },
    { id = 'shop_247_03', mode = 'shop', coords = vector4(26.10, -1347.27, 29.50, 0.0) },
    { id = 'shop_247_04', mode = 'shop', coords = vector4(-3038.9, 585.9, 7.9, 0.0) },
    { id = 'shop_247_05', mode = 'shop', coords = vector4(-3241.9, 1001.4, 12.8, 0.0) },
    { id = 'shop_247_06', mode = 'shop', coords = vector4(547.4, 2671.7, 42.1, 0.0) },
    { id = 'shop_247_07', mode = 'shop', coords = vector4(1961.4, 3740.6, 32.3, 0.0) },
    { id = 'shop_247_08', mode = 'shop', coords = vector4(2678.9, 3280.6, 55.2, 0.0) },
    { id = 'shop_247_09', mode = 'shop', coords = vector4(1729.2, 6414.1, 35.0, 0.0) },
    { id = 'shop_247_10', mode = 'shop', coords = vector4(1135.8, -982.2, 46.4, 0.0) },
    { id = 'shop_247_11', mode = 'shop', coords = vector4(-1222.9, -906.9, 12.3, 0.0) },
    { id = 'shop_247_12', mode = 'shop', coords = vector4(-1487.5, -379.1, 40.1, 0.0) },
    { id = 'shop_247_13', mode = 'shop', coords = vector4(-2968.2, 390.9, 15.0, 0.0) },
    { id = 'shop_247_14', mode = 'shop', coords = vector4(1166.0, 2708.9, 38.1, 0.0) },
    { id = 'shop_247_15', mode = 'shop', coords = vector4(1392.5, 3604.6, 34.9, 0.0) },
    { id = 'shop_247_16', mode = 'shop', coords = vector4(-48.5, -1757.5, 29.4, 0.0) },
    { id = 'shop_247_17', mode = 'shop', coords = vector4(1163.3, -323.8, 69.2, 0.0) },
    { id = 'shop_247_18', mode = 'shop', coords = vector4(-707.5, -914.2, 19.2, 0.0) },
    { id = 'shop_247_19', mode = 'shop', coords = vector4(-1820.5, 792.5, 138.1, 0.0) },
    { id = 'shop_247_20', mode = 'shop', coords = vector4(1698.3, 4924.4, 42.0, 0.0) },
}
```

config\_items.lua

```lua
-----------------------------------------------------------
-- SHOP ITEMS CONFIGURATION (Per Mode)
-----------------------------------------------------------
-- Each shop mode has its own items configuration
-- stock: -1 = unlimited, or set a number for limited stock
-- chance: 0-100 (percentage chance to appear when rotation is enabled)
--         100 = always appears, 50 = 50% chance, etc.
--         Omit or set to 100 for guaranteed items
-- prices: Define prices for each payment method
--         Only include the payment methods you want to accept
--         For item payments, the value is the quantity required

return {
    -----------------------------------------------------------
    -- BLACKMARKET ITEMS
    -----------------------------------------------------------
    blackmarket = {
        -- WEAPONS
        { item = 'WEAPON_GLOCK19X', label = 'GLOCK 19X', category = 'weapons', description = 'Compact 9mm sidearm', stock = -1, chance = 85, prices = { black_money = 12000, diamonds = 4 } },
        { item = 'WEAPON_MPX', label = 'MPX', category = 'weapons', description = 'Compact 9mm SMG', stock = -1, chance = 70, prices = { black_money = 25000, diamonds = 8 } },
        { item = 'WEAPON_DDM4V5', label = 'DDM4V5', category = 'weapons', description = '5.56 rifle platform', stock = 8, chance = 55, prices = { black_money = 45000, diamonds = 12 } },
        { item = 'WEAPON_MICRODRACO', label = 'Micro Draco', category = 'weapons', description = 'Compact rifle platform', stock = 6, chance = 50, prices = { black_money = 30000, diamonds = 10 } },

        -- AMMUNITION
        { item = 'ammo-9', label = '9mm Ammo', category = 'ammo', description = 'Box of 9mm rounds', stock = -1, chance = 100, prices = { black_money = 200, diamonds = 1 } },
        { item = 'ammo-rifle', label = 'Rifle Ammo', category = 'ammo', description = 'Box of 5.56 rounds', stock = -1, chance = 100, prices = { black_money = 500, diamonds = 1 } },
        { item = 'ammo-rifle2', label = 'Rifle Ammo (7.62)', category = 'ammo', description = 'Box of 7.62 rounds', stock = -1, chance = 90, prices = { black_money = 600, diamonds = 1 } },

        -- ATTACHMENTS
        { item = 'at_flashlight', label = 'Flashlight', category = 'attachments', description = 'Tactical weapon light', stock = -1, chance = 90, prices = { black_money = 2000, diamonds = 1 } },
        { item = 'at_suppressor_light', label = 'Rifle Suppressor', category = 'attachments', description = 'Reduces weapon noise', stock = -1, chance = 70, prices = { black_money = 6000, diamonds = 2 } },
        { item = 'at_grip', label = 'Grip', category = 'attachments', description = 'Improves weapon handling', stock = -1, chance = 75, prices = { black_money = 2500, diamonds = 1 } },

        -- TOOLS
        { item = 'lockpick', label = 'Lockpick', category = 'tools', description = 'Opens locked doors', stock = -1, chance = 100, prices = { black_money = 400, diamonds = 1 } },
        { item = 'wire_cutter', label = 'Wire Cutter', category = 'tools', description = 'Cuts wires and restraints', stock = -1, chance = 85, prices = { black_money = 1500, diamonds = 1 } },
        { item = 'drill', label = 'Drill', category = 'tools', description = 'Electric drill for breaking safes', stock = -1, chance = 60, prices = { black_money = 8000, diamonds = 3 } },
        { item = 'thermite', label = 'Thermite', category = 'tools', description = 'Burns through metal', stock = 20, chance = 45, prices = { black_money = 6000, diamonds = 2 } },
        { item = 'electronickit', label = 'Electronic Kit', category = 'tools', description = 'Hacking and electronics tools', stock = -1, chance = 70, prices = { black_money = 4000, diamonds = 2 } },

        -- ARMOR
        { item = 'armour', label = 'Body Armor', category = 'armor', description = 'Standard bulletproof vest', stock = -1, chance = 80, prices = { black_money = 4000, diamonds = 2 } },

        -- DRUGS / SUBSTANCES
        { item = 'opium_pooch', label = 'Promethazine', category = 'drugs', description = 'Bagged promethazine', stock = -1, chance = 80, prices = { black_money = 1200, diamonds = 1 } },
        { item = 'coke_pooch', label = 'Bagged Coke', category = 'drugs', description = 'Cocaine in baggies', stock = -1, chance = 60, prices = { black_money = 4000, diamonds = 2 } },
        { item = 'meth_pooch', label = 'Bagged Meth', category = 'drugs', description = 'Methamphetamine in baggies', stock = -1, chance = 60, prices = { black_money = 3000, diamonds = 2 } },
        { item = 'xanax', label = 'Xanax', category = 'drugs', description = 'Prescription sedative', stock = -1, chance = 70, prices = { black_money = 1500, diamonds = 1 } },
        { item = 'heroin', label = 'Heroin', category = 'drugs', description = 'Processed heroin', stock = -1, chance = 55, prices = { black_money = 3500, diamonds = 2 } },
        { item = 'crack', label = 'Crack', category = 'drugs', description = 'Processed crack cocaine', stock = -1, chance = 65, prices = { black_money = 2500, diamonds = 1 } },
        { item = 'fentanyl', label = 'Fentanyl', category = 'drugs', description = 'Potent synthetic opioid', stock = -1, chance = 40, prices = { black_money = 5000, diamonds = 3 } },
        { item = 'marijuana', label = 'Marijuana', category = 'drugs', description = 'Processed cannabis', stock = -1, chance = 75, prices = { black_money = 800, diamonds = 1 } },

        -- MISCELLANEOUS
        { item = 'radio', label = 'Radio', category = 'misc', description = 'Encrypted communication device', stock = -1, chance = 90, prices = { black_money = 1600, diamonds = 1 } },
        { item = 'binoculars', label = 'Binoculars', category = 'misc', description = 'Long-range observation', stock = -1, chance = 85, prices = { black_money = 800, diamonds = 1 } },
        { item = 'handcuffs', label = 'Handcuffs', category = 'misc', description = 'Restraint device', stock = -1, chance = 75, prices = { black_money = 600, diamonds = 1 } },
        { item = 'burnerphone', label = 'Burner Phone', category = 'misc', description = 'Disposable untraceable phone', stock = -1, chance = 80, prices = { black_money = 2000, diamonds = 1 } },
    },

    -----------------------------------------------------------
    -- GENERAL STORE ITEMS
    -----------------------------------------------------------
    shop = {
        { item = 'water', label = 'Water Bottle', category = 'misc', description = 'Stay hydrated', stock = -1, chance = 100, prices = { cash = 10, bank = 10 } },
        { item = 'bread', label = 'Bread', category = 'misc', description = 'Fresh bread', stock = -1, chance = 100, prices = { cash = 15, bank = 15 } },
        { item = 'sandwich', label = 'Sandwich', category = 'misc', description = 'Ham and cheese sandwich', stock = -1, chance = 100, prices = { cash = 25, bank = 25 } },
        { item = 'lockpick', label = 'Lockpick', category = 'tools', description = 'Opens locked doors', stock = -1, chance = 100, prices = { cash = 500, bank = 550 } },
        { item = 'radio', label = 'Radio', category = 'misc', description = 'Communication device', stock = -1, chance = 100, prices = { cash = 2000, bank = 2200 } },
        { item = 'binoculars', label = 'Binoculars', category = 'misc', description = 'Long-range observation', stock = -1, chance = 100, prices = { cash = 1000, bank = 1100 } },
    },

    -----------------------------------------------------------
    -- GUN SHOP ITEMS (Ammunation)
    -----------------------------------------------------------
    -- gunshop = {
    --     -- WEAPONS (legal firearms)
    --     { item = 'WEAPON_PISTOL', label = 'Pistol', category = 'weapons', description = 'A standard 9mm handgun', stock = -1, chance = 100, prices = { cash = 7500, bank = 7500 } },
    --     { item = 'WEAPON_COMBATPISTOL', label = 'Combat Pistol', category = 'weapons', description = 'Compact and lightweight pistol', stock = -1, chance = 100, prices = { cash = 10000, bank = 10000 } },
    --     { item = 'WEAPON_PUMPSHOTGUN', label = 'Pump Shotgun', category = 'weapons', description = 'Powerful pump-action shotgun', stock = -1, chance = 100, prices = { cash = 45000, bank = 45000 } },
    --     { item = 'WEAPON_KNIFE', label = 'Knife', category = 'weapons', description = 'Sharp combat knife', stock = -1, chance = 100, prices = { cash = 750, bank = 750 } },

    --     -- AMMUNITION
    --     { item = 'ammo-9', label = '9mm Ammo', category = 'ammo', description = 'Box of 9mm rounds (24 bullets)', stock = -1, chance = 100, prices = { cash = 350, bank = 350 } },
    --     { item = 'ammo-shotgun', label = 'Shotgun Shells', category = 'ammo', description = 'Box of shotgun shells (12 shells)', stock = -1, chance = 100, prices = { cash = 500, bank = 500 } },

    --     -- ATTACHMENTS
    --     { item = 'at_flashlight', label = 'Flashlight', category = 'attachments', description = 'Tactical weapon light', stock = -1, chance = 100, prices = { cash = 3500, bank = 3500 } },

    --     -- ARMOR
    --     { item = 'armor', label = 'Body Armor', category = 'armor', description = 'Standard bulletproof vest', stock = -1, chance = 100, prices = { cash = 6500, bank = 6500 } },
    -- },

    -----------------------------------------------------------
    -- PHARMACY ITEMS
    -----------------------------------------------------------
    -- pharmacy = {
    --     { item = 'bandage', label = 'Bandage', category = 'misc', description = 'Basic first aid', stock = -1, chance = 100, prices = { cash = 100, bank = 100 } },
    --     { item = 'firstaid', label = 'First Aid Kit', category = 'misc', description = 'Complete first aid kit', stock = -1, chance = 100, prices = { cash = 500, bank = 500 } },
    --     { item = 'painkillers', label = 'Painkillers', category = 'misc', description = 'Pain relief medication', stock = -1, chance = 100, prices = { cash = 200, bank = 200 } },
    -- },

    -----------------------------------------------------------
    -- PAWN SHOP ITEMS
    -----------------------------------------------------------
    -- pawnshop = {
    --     { item = 'radio', label = 'Radio', category = 'misc', description = 'Second-hand communication device', stock = -1, chance = 100, prices = { cash = 1500, bank = 1500 } },
    --     { item = 'binoculars', label = 'Binoculars', category = 'misc', description = 'Used binoculars', stock = -1, chance = 100, prices = { cash = 750, bank = 750 } },
    --     { item = 'phone', label = 'Phone', category = 'misc', description = 'Refurbished phone', stock = -1, chance = 100, prices = { cash = 500, bank = 500 } },
    -- },
}


```
