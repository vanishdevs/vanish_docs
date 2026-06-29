---
description: >-
  The server exports for reading leaderboard data from your own resources,
  plus every command the script registers.
---

# Exports

## Server

### GetLeaderboard

Fetches a page of the robbery leaderboard. Returns the same payload the NUI reads, so you can use it from any server-side resource.

```lua
local data = exports.vanish_robbing:GetLeaderboard(metric, page, limit, search)
```

| Argument | Type | Notes |
| --- | --- | --- |
| `metric` | string / nil | Sort column: `successful`, `failed`, `total`, `cash`, `items`, `last`. Defaults to `rankingMetric` in config. |
| `page` | number / nil | Page number, 1-indexed. Defaults to `1`. |
| `limit` | number / nil | Rows per page. Capped by `maxLimit` in config. Defaults to `defaultLimit`. |
| `search` | string / nil | Filter by player name or identifier. Pass `nil` for no filter. |

Returns a table:

```lua
{
    rows  = { ... },   -- array of player row tables
    page  = 1,
    limit = 25,
    total = 142,       -- total matching rows in the database
    pages = 6,
    metric = 'successful',
    search = '',
}
```

Each row in `rows` contains:

| Field | Description |
| --- | --- |
| `position` | Absolute position in the current page (e.g. 1 on page 1, 26 on page 2) |
| `identifier` | Player identifier |
| `name` | Display name |
| `rank` | Numeric rank (1 = top) |
| `successful` | Successful robbery count |
| `failed` | Failed robbery count |
| `total` | Total attempts |
| `cashStolen` | Total cash-type items stolen |
| `itemsStolen` | Total non-cash items stolen |
| `successRate` | Percentage of attempts that succeeded |
| `lastRobberyAt` | Timestamp of last robbery |
| `breakdown` | Array of `{ name, label, count, cash }` per stolen item type |

```lua
-- Fetch the top 10 robbers sorted by cash stolen
local data = exports.vanish_robbing:GetLeaderboard('cash', 1, 10)
for _, row in ipairs(data.rows) do
    print(row.name, row.cashStolen)
end
```

---

### GetPlayerRobberyStats

Fetches the robbery stats for a single player.

```lua
local stats = exports.vanish_robbing:GetPlayerRobberyStats(identifierOrSource)
```

| Argument | Type | Notes |
| --- | --- | --- |
| `identifierOrSource` | number or string | A player server ID (number), or a raw identifier string (e.g. `'license:xxxx'`). |

Returns the same row table as above, or `nil` if the player has no recorded robberies.

```lua
-- By server ID
local stats = exports.vanish_robbing:GetPlayerRobberyStats(source)

-- By identifier
local stats = exports.vanish_robbing:GetPlayerRobberyStats('license:abc123')

if stats then
    print(stats.name, stats.successful, stats.failed)
end
```

---

## Commands

| Command | Who can use | Description |
| --- | --- | --- |
| `/rob` | Everyone | Rob the nearest player. Only registered when `input.method` is `command` or `both`. The name is set by `input.command.name`. |
| `/robleaderboard` | Everyone | Open the robbery leaderboard UI. Only registered when `leaderboard.enableCommand = true`. The name is set by `leaderboard.commandName`. |
| `/togglerobbery` | `admin.toggleRobbery` ace | Toggle the robbery system on or off for the whole server. |
| `/clearrobcooldown [id]` | `admin.clearCooldown` ace | Clear both the robber and victim cooldowns for the given player server ID. |
| `/resetrobleaderboard` | `admin.resetLeaderboard` ace | Wipe the `robbery_stats` table and reset all counters. |

Ace permissions for each admin command are set in the `admin` block of `shared/config.lua`. Set a value to `false` to allow anyone to run that command.
