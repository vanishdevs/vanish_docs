---
description: >-
  Air-dropped supply crates with a cargo plane flyover, tiered loot, contested
  unlock timers and combat zones. Drops can fire on a timer, be spawned by
  admins, or be called in by players with a supply s
icon: parachute-box
---

# Crate Drops

## Features

* Three ways to trigger a drop: automatic drops on a timer, admin commands, and a supply signal item players can use,
* A cargo plane flies in over the map and releases the crate, which falls under a parachute with smoke trailing behind it,
* A ground flare and smoke marker shows where the crate is going to land before it gets there,
* Five loot tiers (common, uncommon, rare, epic, legendary), each with its own loot table, spawn chance, blip colour and smoke colour,
* Unlock delay so the crate cannot be opened the moment it lands, which gives everyone a window to fight over it,
* Optional skillcheck per tier before a crate will open,
* Loot is held in a real stash, so several players can keep pulling items until the crate is empty,
* Combat zones drawn around every active drop and synced to all players for other scripts to read,
* Predefined drop locations or fully random placement anywhere on the map,
* Supply signal cooldowns, item validation and coordinate checks to stop players abusing the signal,
* vanish\_gangs integration that notifies gang members when a crate goes live,
* Discord webhook logging for drops created, opened, looted, expired, admin actions and signal use,
* Built on a bridge system so it runs on ESX, QB and QBox with ox, qb or qs inventory
