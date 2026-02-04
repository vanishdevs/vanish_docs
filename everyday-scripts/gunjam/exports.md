---
description: >-
  List of exports required for developers to integrate this script into their
  own resources.
---

# Exports

**Client**

The gun jam system provides client-side exports for checking and managing weapon jam states.

**Helper Exports (Recommended)**\
Simple function wrappers for common operations:

**IsWeaponJammed**

```lua
local isJammed = exports.vanish_gunjam:IsWeaponJammed()
```

Returns: `boolean` - Whether the current weapon is jammed

**GetJammedWeapon**

```lua
local jammedWeapon = exports.vanish_gunjam:GetJammedWeapon()
```

Returns: `number|nil` - The hash of the currently jammed weapon or nil

**ClearJam**

```lua
exports.vanish_gunjam:ClearJam()
```

Returns: `void` - Forces clearing of the current jam (bypasses normal mechanics)

**GetJamChance**

```lua
local chance = exports.vanish_gunjam:GetJamChance(weaponHash)
```

Returns: `number` - The jam chance percentage for the specified weapon hash

**Server**\
No server-side exports available. The server uses callback system for whitelist checks.
