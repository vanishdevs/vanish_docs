---
description: >-
  List of exports required for developers to integrate this script into their
  own resources.
---

# Exports

### Client

No client-side exports available.

### Server

The weapon editor exposes a small set of helper exports for reading the scanner cache and install state.

{% hint style="info" %}
There are no direct module exports. The underlying services (`WeaponScanner`, `InstallerService`, `MetaEditorService`, `BackupService`, `AuditService`) are intentionally scoped to this resource so that all writes — installs, meta edits, rollbacks — go through the audited UI flow.
{% endhint %}

#### Helper Exports

Simple function wrappers for reading scanner and installer state.

**ScanWeapons**

```lua
local result = exports.vanish_weaponeditor:ScanWeapons(forced)
```

**Parameters**

| Name     | Type      | Description                                                                          |
| -------- | --------- | ------------------------------------------------------------------------------------ |
| `forced` | `boolean` | Optional. When `true`, bypasses the `cacheResultsMs` cache and forces a full rescan. |

**Returns:** Full scan result table

```lua
{
    resources    = { ... },     -- Array of resource summaries with their meta files
    weapons      = { ... },     -- Map of weaponName -> full weapon data
    components   = { ... },     -- Map of componentName -> component data
    totalWeapons = 17,          -- Count of unique weapons found
    totalComponents = 42,       -- Count of unique components found
    totalResources  = 5,        -- Count of resources containing weapon metas
    scannedAt    = 1748383200   -- Unix timestamp of the scan
}
```

**GetWeaponList**

```lua
local list = exports.vanish_weaponeditor:GetWeaponList(resourceFilter)
```

**Parameters**

| Name             | Type     | Description                                                       |
| ---------------- | -------- | ----------------------------------------------------------------- |
| `resourceFilter` | `string` | Optional. When set, only weapons from this resource are returned. |

**Returns:** Array of weapon summary objects, sorted by category then name

```lua
{
    {
        name           = "WEAPON_AUTOPISTOL_A",
        model          = "w_pi_autopistol_a",
        category       = "Pistol",
        resource       = "vanish_weapons",
        sourceFile     = "weapons_pistols.meta",
        damage         = 24,
        clipSize       = 15,
        range          = 110,
        accuracySpread = 3.0,
        fireRate       = 7.0,
        componentCount = 4
    }
}
```

**GetWeaponDetail**

```lua
local weapon = exports.vanish_weaponeditor:GetWeaponDetail(weaponName)
```

**Parameters**

| Name         | Type     | Description                                    |
| ------------ | -------- | ---------------------------------------------- |
| `weaponName` | `string` | The full weapon name (e.g. `"WEAPON_PISTOL"`). |

**Returns:** Table with full weapon data including linked components and attach points, or `nil` if not found

```lua
{
    name             = "WEAPON_PISTOL",
    model            = "w_pi_pistol",
    category         = "Pistol",
    resource         = "weapon_pistol",
    sourceFile       = "weapons.meta",
    damage           = 26,
    clipSize         = 12,
    range            = 120,
    accuracySpread   = 3.5,
    timeBetweenShots = 0.166,
    audio            = "AUDIO_PISTOL",
    group            = "GROUP_PISTOL",
    ammoType         = "AMMO_PISTOL",
    linkedComponents = { ... },  -- Array of component objects
    attachPoints     = { ... }   -- Array of attach point objects
}
```

**IsWeaponInstalled**

```lua
local installed = exports.vanish_weaponeditor:IsWeaponInstalled(weaponName)
```

**Parameters**

| Name         | Type     | Description                             |
| ------------ | -------- | --------------------------------------- |
| `weaponName` | `string` | The full weapon name. Case-insensitive. |

**Returns:** `boolean` — whether the weapon is currently registered in the active inventory system (`ox_inventory`, `qb-inventory`, or `qs-inventory`).

### Usage Examples

#### Simple Usage

```lua
-- Force a fresh scan and report stats
local result = exports.vanish_weaponeditor:ScanWeapons(true)
print(('Found %d weapons across %d resources'):format(result.totalWeapons, result.totalResources))

-- List every weapon from a specific resource
local weapons = exports.vanish_weaponeditor:GetWeaponList('vanish_weapons')
for _, w in ipairs(weapons) do
    print(w.name, w.category, w.damage)
end

-- Look up one weapon and read its components
local detail = exports.vanish_weaponeditor:GetWeaponDetail('WEAPON_PISTOL')
if detail then
    for _, comp in ipairs(detail.linkedComponents or {}) do
        print(comp.name, comp.attachPoint, comp.installed)
    end
end

-- Gate logic on whether a weapon is installed in the inventory
if exports.vanish_weaponeditor:IsWeaponInstalled('WEAPON_AUTOPISTOL_A') then
    print('Weapon is registered in the inventory')
end
```

#### Advanced Usage

```lua
-- Build a per-category summary across all scanned resources
local result = exports.vanish_weaponeditor:ScanWeapons(false)
local byCategory = {}
for _, weapon in pairs(result.weapons) do
    byCategory[weapon.category] = (byCategory[weapon.category] or 0) + 1
end
for category, count in pairs(byCategory) do
    print(category, count)
end

-- Sync your own item registry against installed weapons
local list = exports.vanish_weaponeditor:GetWeaponList()
for _, weapon in ipairs(list) do
    local installed = exports.vanish_weaponeditor:IsWeaponInstalled(weapon.name)
    if not installed then
        print('Missing from inventory:', weapon.name)
    end
end
```
