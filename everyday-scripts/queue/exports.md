---
description: >-
  Server exports for reading the queue and managing priority from your own
  resources, plus the admin commands the script registers.
---

# Exports

All exports are server side. Identifiers are the player's license identifier (for example `license:abc123...`).

## Reading the queue

### GetQueuePosition

```lua
local position = exports.vanish_queue:GetQueuePosition(identifier)
```

Returns: the player's place in line, or nil if they are not queued.

### GetQueueSize

```lua
local size = exports.vanish_queue:GetQueueSize()
```

Returns: the number of players currently waiting.

### GetQueueList

```lua
local list = exports.vanish_queue:GetQueueList()
```

Returns: an array of the players in the queue, each with their position, name and priority points.

### IsPlayerInQueue

```lua
local queued = exports.vanish_queue:IsPlayerInQueue(identifier)
```

Returns: `true` if the player is currently in the queue.

### GetPlayerPriority

```lua
local points = exports.vanish_queue:GetPlayerPriority(identifier)
```

Returns: the player's total priority points.

## Managing priority

These all need `database.enabled = true` in the config. They return `false` plus a reason if the database is off.

### AddPriority

```lua
local ok = exports.vanish_queue:AddPriority(identifier, points, category)
```

Adds priority points to a player. `category` is optional and labels where the points came from (it shows on the player's priority breakdown). Returns `true` on success.

### RemovePriority

```lua
local ok = exports.vanish_queue:RemovePriority(identifier, category)
```

Removes priority from a player. Pass a `category` to remove just that source, or leave it out to remove all of their priority. Returns `true` on success.

### RemoveAllPriority

```lua
local ok = exports.vanish_queue:RemoveAllPriority(identifier)
```

Removes every priority entry for a player. Returns `true` on success.

### Example

```lua
-- Give a player 50 priority points tagged as a reward
exports.vanish_queue:AddPriority('license:abc123', 50, 'reward')

-- Take that reward back later
exports.vanish_queue:RemovePriority('license:abc123', 'reward')
```

## Commands

| Command | Arguments | Description |
| --- | --- | --- |
| `/queue:add` | `identifier` `points` `[category]` | Add priority points to a player. |
| `/queue:remove` | `identifier` `[category]` | Remove priority from a player. Omit the category to remove all of it. |
| `/queue:list` | none | Show everyone currently in the queue. |
| `/queue:clear` | none | Clear the whole queue. |

Access to each command is set by the `admin` block in `config/config.lua`. These commands need `database.enabled = true` to change stored priority.
