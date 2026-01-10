---
description: >-
  List of exports required for developers to integrate this script into their
  own resources.
---

# Exports

**Client**&#x20;

No client-side exports available.

***

**Server**&#x20;

The turf system provides simple function exports for interacting with turf wars and gang statistics.

#### Available Exports

**GetLeaderboardData**

```lua
local leaderboard = exports.vanish_turfs:GetLeaderboardData()
```

Returns: Array of gang statistics ordered by wins (descending)

```lua
{
    {
        id = 1,                    -- Gang database ID
        name = "Ballas",          -- Gang name (or label if GetGangLabel is available)
        totalWins = 15,           -- Total turf wars won
        totalBattles = 20,        -- Total turf wars participated in
        logoUrl = "https://..."   -- Gang logo URL (empty string if not set)
    },
    -- ... more gangs
}
```

**GetActiveTurfs**

```lua
local activeTurfs = exports.vanish_turfs:GetActiveTurfs()
```

Returns: Array of currently active turf wars

```lua
{
    {
        label = "Grove Street",   -- Zone label
        remaining = 120           -- Remaining seconds until completion
    },
    -- ... more active turfs
}
```

**ForceEndTurf**

```lua
local success = exports.vanish_turfs:ForceEndTurf(zoneLabel)
```

**Parameters:**

* `zoneLabel` (string) - The zone label to force end

Returns: boolean - Whether the turf was successfully ended

**Note:** This will trigger reward distribution, stats recording, and respawn logic just like a normal turf completion.

**GetGangStats**

```lua
local gangStats = exports.vanish_turfs:GetGangStats(gangName)
```

**Parameters:**

* `gangName` (string) - The gang name to look up

Returns: Gang statistics object or nil if gang not found

```lua
{
    id = 1,                    -- Gang database ID
    name = "Ballas",          -- Gang name (or label if GetGangLabel is available)
    totalWins = 15,           -- Total turf wars won
    totalBattles = 20,        -- Total turf wars participated in
    logoUrl = "https://..."   -- Gang logo URL (empty string if not set)
}
```

***

#### Usage Examples

**Check Active Turfs**

```lua
local activeTurfs = exports.vanish_turfs:GetActiveTurfs()

if #activeTurfs > 0 then
    print(('There are %d active turfs'):format(#activeTurfs))
    for _, turf in ipairs(activeTurfs) do
        print(('- %s: %ds remaining'):format(turf.label, turf.remaining))
    end
else
    print('No active turfs')
end
```

**Get Gang Statistics**

```lua
local gangStats = exports.vanish_turfs:GetGangStats('ballas')

if gangStats then
    local winRate = gangStats.totalBattles > 0 
        and (gangStats.totalWins / gangStats.totalBattles * 100) 
        or 0
    
    print(('%s: %d wins / %d battles (%.1f%% win rate)'):format(
        gangStats.name, 
        gangStats.totalWins, 
        gangStats.totalBattles, 
        winRate
    ))
else
    print('Gang not found in statistics')
end
```

**Display Leaderboard**

```lua
local leaderboard = exports.vanish_turfs:GetLeaderboardData()

print('=== Turf War Leaderboard ===')
for i, gang in ipairs(leaderboard) do
    print(('%d. %s - %d wins (%d battles)'):format(
        i, 
        gang.name, 
        gang.totalWins, 
        gang.totalBattles
    ))
end
```
