---
description: >-
  List of exports required for developers to integrate this script into their
  own resources.
---

# Exports

**Client**

The shop system provides client-side exports for opening/closing the UI and checking its state.

**Helper Exports (Recommended)**\
Simple function wrappers for common operations:

**OpenShop**

```lua
exports.vanish_shops:OpenShop(locationId)
```

Returns: void - Opens the shop UI; optional `locationId` preselects a shop before opening.

**CloseShop**

```lua
exports.vanish_shops:CloseShop()
```

Returns: void - Closes the shop UI and releases focus.

**IsShopOpen**

```lua
local isOpen = exports.vanish_shops:IsShopOpen()
```

Returns: boolean - Whether the shop UI is currently open.

**Server**

Server-side exports for querying and managing shop stock values.

**Stock Exports**\
Functions to query and mutate inventory:

**GetStock**

```lua
local stock = exports.vanish_shops:GetStock(itemName)
```

Returns: number|-1|table - Stock for a single item (number or -1 for unlimited) or a table of all active items when `itemName` is nil.

**SetStock**

```lua
local ok = exports.vanish_shops:SetStock(itemName, amount)
```

Returns: boolean - Success status; sets absolute stock for the specified item.

**AddStock**

```lua
local ok = exports.vanish_shops:AddStock(itemName, amount)
```

Returns: boolean - Success status; increments stock for the specified item (no effect on unlimited items).

**RestockAll**

```lua
exports.vanish_shops:RestockAll()
```

Returns: void - Resets every item’s stock to its configured default values.
