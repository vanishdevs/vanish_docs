---
description: >-
  Exports for reading war and gang stats, controlling the phone, and ending wars
  from your own resources. Plus the commands the script registers.
---

# Exports

## Server

### GetLeaderboard

```lua
local board = exports.vanish_gangphone:GetLeaderboard(sortBy, limit)
```

Returns the gang leaderboard. `sortBy` and `limit` are optional. Each row includes the gang name and label, total wars, wins, losses, kills, deaths, K/D ratio, win rate and net profit, which is handy for building your own leaderboard UI or web panel.

### GetGangStats

```lua
local gangStats = exports.vanish_gangphone:GetGangStats(gangName)
```

Returns the war stats for a single gang (wars, wins, losses, kills, deaths, money won and lost).

### GetActiveWars

```lua
local wars = exports.vanish_gangphone:GetActiveWars()
```

Returns an array of the wars happening right now, each with both gangs, their current kills, the wager and the kill target.

### IsGangInWar

```lua
local atWarLimit = exports.vanish_gangphone:IsGangInWar(gangName)
```

Returns `true` if the gang has hit its limit of concurrent wars.

### ForceEndWar

```lua
local ended = exports.vanish_gangphone:ForceEndWar(warId)
```

Ends a war by its ID. Returns `true` if a war was found and ended. This is the same action as the `/endwar` command.

## Client

### OpenPhone / ClosePhone

```lua
exports.vanish_gangphone:OpenPhone()
exports.vanish_gangphone:ClosePhone()
```

Opens or closes the phone. `OpenPhone` is the export you wire a usable phone item to.

### IsPhoneOpen

```lua
local open = exports.vanish_gangphone:IsPhoneOpen()
```

Returns `true` while the phone is open.

### GetPlayerGangData

```lua
local gang = exports.vanish_gangphone:GetPlayerGangData()
```

Returns the local player's current gang data (name, rank and so on).

### Markers

```lua
exports.vanish_gangphone:EnableMarkers()
exports.vanish_gangphone:DisableMarkers()
exports.vanish_gangphone:ToggleMarkers()
local markers = exports.vanish_gangphone:GetWarMarkers()
```

Turn the ally/enemy war markers on or off, or read the current marker state.

## Commands

| Command | Arguments | Description |
| --- | --- | --- |
| `/gangphone` | none | Opens the phone. The name comes from `phone.command` in the config. |
| `/endwar` | `warId` | Force ends a war. Restricted by `admin.permissions.forceEndWar`. |
| `/listwars` | none | Lists every active war. Restricted by `admin.permissions.listWars`. |
