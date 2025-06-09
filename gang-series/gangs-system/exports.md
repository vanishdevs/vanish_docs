---
description: >-
  List of exports required for developers to integrate this script into their
  own resources.
---

# Exports

## Client

| Export Name | Description                                        | Parameters                                                 | Returns |
| ----------- | -------------------------------------------------- | ---------------------------------------------------------- | ------- |
| `uiDisplay` | Toggles the visibility of the gang information UI. | `state` (boolean) – Show (`true`) or hide (`false`) the UI | `void`  |

## Server

The following functions originate from the exported `Gangs` table, which serves as the main interface for accessing and interacting with gang-related data.

| Function                                 | Description                                                                | Parameters                           | Returns                                          |
| ---------------------------------------- | -------------------------------------------------------------------------- | ------------------------------------ | ------------------------------------------------ |
| `Gangs.GetPlayerGang(playerId)`          | Gets the current gang data of a player (name, rank, label, leader status). | `playerId` (number)                  | `table` – `{ name, rank, rankString, isLeader }` |
| `Gangs.GetMembers(gangName)`             | Returns all members of a gang with their name, rank, and identifier.       | `gangName` (string)                  | `table` – Member list                            |
| `Gangs.DoesGangExist(gangName)`          | Checks if a gang exists.                                                   | `gangName` (string)                  | `boolean`                                        |
| `Gangs.DoesRankExist(key)`               | Checks if a rank exists by unique key (`gangName_rankIndex`).              | `key` (string)                       | `boolean`                                        |
| `Gangs.IsLeadershipRank(gangName, rank)` | Checks if a rank is the leadership rank of the gang.                       | `gangName` (string), `rank` (number) | `boolean`                                        |
| `Gangs.GetGangLabel(gangName)`           | Returns the display label of a gang.                                       | `gangName` (string)                  | `string`                                         |

