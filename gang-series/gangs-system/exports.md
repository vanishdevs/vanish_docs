---
description: >-
  Stable helper exports for integrating other resources with vanish_gangs, plus
  server events you can listen for.
---

# Exports

## Client

No client-side exports are available.

## Server

These helper exports are the stable surface intended for normal integrations.

### GetPlayerGang

```lua
local playerGang = exports.vanish_gangs:GetPlayerGang(playerId)
```

Returns: a table with the player's gang name, rank, display rank, rank icon,
rank color and leader state, or nil if they are not in a gang.

### GetPlayerRank

```lua
local rank = exports.vanish_gangs:GetPlayerRank(playerId)
```

Returns: the player's numeric rank, or nil if they are not in a gang.

### IsPlayerInGang

```lua
local inGang = exports.vanish_gangs:IsPlayerInGang(playerId, gangName)
```

`gangName` is optional. Leave it out to check whether the player is in any gang.
Pass a gang name to check membership in that specific gang.

Returns: `true` or `false`.

### DoesGangExist

```lua
local exists = exports.vanish_gangs:DoesGangExist(gangName)
```

Returns: `true` when the gang exists.

### GetGangLabel

```lua
local label = exports.vanish_gangs:GetGangLabel(gangName)
```

Returns: the display label for a gang, or nil if the gang does not exist.

### GetGang

```lua
local gang = exports.vanish_gangs:GetGang(gangName)
```

Returns: the gang record, or nil if the gang does not exist.

### GetAllGangs

```lua
local gangs = exports.vanish_gangs:GetAllGangs()
```

Returns: every gang keyed by gang name.

### GetGangMembers

```lua
local members = exports.vanish_gangs:GetGangMembers(gangName)
```

Returns: an array of member objects for the gang.

### GetGangRanks

```lua
local ranks = exports.vanish_gangs:GetGangRanks(gangName)
```

Returns: an array of ranks sorted from lowest to highest.

### GetGangRelationship

```lua
local relationship = exports.vanish_gangs:GetGangRelationship(gangA, gangB)
```

Returns: `'allied'`, `'neutral'` or `'rival'`.

## Examples

### Gate a feature to gang members

```lua
if not exports.vanish_gangs:IsPlayerInGang(source) then
    return
end

local gang = exports.vanish_gangs:GetPlayerGang(source)
print(('Player is in %s as rank %s'):format(gang.name, gang.rankString))
```

### Check diplomacy

```lua
local relation = exports.vanish_gangs:GetGangRelationship('ballas', 'vagos')

if relation == 'rival' then
    print('These gangs are rivals')
end
```

## Server Events

Listen from another server resource with `AddEventHandler`.

```lua
AddEventHandler('vanish_gangs:server:gangCreated', function(gangName, label, createdByIdentifier)
    -- a gang was created
end)

AddEventHandler('vanish_gangs:server:gangDeleted', function(gangName)
    -- a gang was deleted
end)

AddEventHandler('vanish_gangs:server:playerJoinedGang', function(source, identifier, gangName, rank, reason)
    -- a player joined a gang
end)

AddEventHandler('vanish_gangs:server:playerLeftGang', function(source, identifier, gangName, oldRank, reason)
    -- a player left or was removed from a gang
end)

AddEventHandler('vanish_gangs:server:rankChanged', function(source, identifier, gangName, oldRank, newRank, changedBy)
    -- a player's rank changed
end)
```

{% hint style="info" %}
`source` is only reliable while the player is online. Store or compare the
`identifier` when you need a lasting key.
{% endhint %}
