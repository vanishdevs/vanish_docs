---
description: >-
  List of exports required for developers to integrate this script into their
  own resources.
---

# Exports

**Client**

No client-side exports available.

***

**Server**

The gang system provides two types of exports: **Helper Exports** (recommended for most use cases) and **Direct Module Exports** (for advanced usage requiring full module access).

### Helper Exports (Recommended)

Simple function wrappers for common operations:

#### `GetPlayerGang`

```lua
local playerGang = exports.vanish_gangs:GetPlayerGang(playerId)
```

**Returns:** Table with gang information or `nil`

```lua
{
    name = "gangName",        -- Gang identifier
    rank = 3,                 -- Numeric rank
    rankString = "Soldier",   -- Rank display label
    isLeader = false          -- Leadership status
}
```

#### `GetGangMembers`

```lua
local members = exports.vanish_gangs:GetGangMembers(gangName)
```

**Returns:** Array of member objects

```lua
{
    {
        identifier = "char1:abc123",
        gangRank = 3,
        title = "John Doe",
        description = "Gang rank: 3",
        icon = "fa-solid fa-user"
    }
}
```

#### `DoesGangExist`

```lua
local exists = exports.vanish_gangs:DoesGangExist(gangName)
```

**Returns:** `boolean` - Whether the gang exists

#### `IsPlayerInGang`

```lua
local inGang = exports.vanish_gangs:IsPlayerInGang(playerId)
```

**Returns:** `boolean` - Whether the player is in any gang

***

### Direct Module Exports (Advanced Usage)

For advanced usage requiring full module access and methods.

#### Cache Module (`Gangs`)

```lua
local cache = exports.vanish_gangs:Gangs()
```

**Direct Data Access:**

* `cache.gangs` - Table of all gangs indexed by gang name
* `cache.ranks` - Table of all ranks indexed by `gangName_rankNumber`

**Available Methods:**

```lua
cache:GetGangData()                           -- Refresh gang data from database
cache:GetRankData()                           -- Refresh rank data from database
cache:DoesGangExist(gangName)                 -- Check if gang exists
cache:DoesGangHaveRanks(gangName)             -- Check if gang has all ranks configured
cache:DoesRankExist(key)                      -- Check if rank key exists (format: "gangName_rankNumber")
cache:DoesRankNameExist(gangName, rankName)   -- Check if rank name exists for gang
cache:DoesRankLabelExist(gangName, rankLabel) -- Check if rank label exists for gang
cache:IsRankWithinInterval(gangName, rank)    -- Validate rank number is within gang's range
cache:IsLeadershipRank(gangName, rank)        -- Check if rank is the leadership rank
cache:GetGangLabel(gangName)                  -- Get gang display label
cache:UpdateGangCache(name)                   -- Update specific gang in cache
cache:RemoveGangFromCache(name)               -- Remove gang from cache
cache:UpdateRankCache(gangName, ranking)      -- Update rank cache for gang
cache:RemoveRankFromCache(gangName, ranking)  -- Remove specific rank from cache
cache:RemoveGangRanksFromCache(gangName)      -- Remove all ranks for gang from cache
cache:GetPlayerGang(playerId)                 -- Get player's gang data
cache:GetMembers(gangName)                    -- Get all members of a gang
```

#### Admin Module (`GangAdmin`)

```lua
local admin = exports.vanish_gangs:GangAdmin()
```

**Available Methods:**

```lua
admin:CreateGang(data)                        -- data = {name, label, leadershipRank, passcode}
admin:CreateRank(data)                        -- data = {gangName, name, label, ranking}
admin:DeleteGang(gangName)                    -- Delete gang and all associated data
admin:DeleteRank(gangName, rankNumber)        -- Delete specific rank
admin:UpdateRankLabel(gangName, rankNumber, rankLabel, rankName) -- Update rank display info
admin:UpdateRankRanking(gangName, rankNumber, rankRanking)       -- Change rank number
admin:SetPasscode(gangName, passcode)         -- Update gang inventory passcode
```

#### Members Module (`GangMember`)

```lua
local members = exports.vanish_gangs:GangMember()
```

**Available Methods:**

```lua
members:ClearInvite(identifier)               -- Clear pending invite for player
members:SendInvite(gangName, playerId)        -- Send gang invite to player
members:AcceptInvite(gangName, playerId)      -- Accept pending invite
members:DeclineInvite(playerId)               -- Decline pending invite
members:InsertPlayer(gangName, gangRank, playerId)           -- Add player to gang
members:RemovePlayer(gangName, playerId, isKicked)           -- Remove player from gang
members:PromotePlayer(identifier, gangName, toRank)          -- Promote player
members:DemotePlayer(identifier, gangName, toRank)           -- Demote player
```

***

### Usage Examples

**Simple Usage:**

```lua
-- Check if player is in a gang
if exports.vanish_gangs:IsPlayerInGang(source) then
    local gangData = exports.vanish_gangs:GetPlayerGang(source)
    print(gangData.name, gangData.rankString)
end

-- Get gang members
local members = exports.vanish_gangs:GetGangMembers('ballas')
```

**Advanced Usage:**

```lua
-- Access the full Cache module
local cache = exports.vanish_gangs:Gangs()

-- Direct access to cached gang data
for gangName, gangData in pairs(cache.gangs) do
    print(gangName, gangData.label, gangData.leadership_rank)
end

-- Use cache methods
if cache:IsLeadershipRank('ballas', 5) then
    print('Player is a leader')
end

-- Administrative operations
local admin = exports.vanish_gangs:GangAdmin()
admin:CreateGang({'newgang', 'New Gang', 5, '1234'})
admin:SetPasscode('ballas', '5678')

-- Member management
local members = exports.vanish_gangs:GangMember()
members:InsertPlayer('ballas', 1, playerId)
members:PromotePlayer(identifier, 'ballas', 3)
```

***
