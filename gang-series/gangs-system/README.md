---
description: >-
  A full gang management system. Admins create and run gangs with custom rank
  ladders, leaders manage their members and a shared stash, and other resources
  can read gang data through a clean export API.
icon: gun
---

# Gangs System

The backbone of the gang series. It handles who is in which gang, their ranks, their shared stash and how gangs relate to each other, and exposes all of it to your other resources so turfs, drug sales, war and anything else can build on top.

## Features

* Create gangs from a template (street gang, motorcycle club, mafia family, cartel) or start blank and build your own,
* Full rank ladders you can create, rename, reorder and delete, each rank with its own icon, colour and permissions,
* Member management for leaders: invite, accept or decline, promote, demote and kick, all gated by rank,
* A shared gang stash locked behind a passcode, with per-rank daily withdrawal limits on both value and item count,
* Gang relationships, so any two gangs can be allied, neutral or rivals,
* A gang health score (0 to 100) with optional admin alerts when a gang starts to fall off,
* Recruitment and an activity log that keeps a record of what each gang has been up to,
* A HUD that shows a player their gang and rank,
* Management points as a single shared spot or per-gang locations, with NPC or marker interaction,
* Admin menu opened by command or keybind, with permissions through ACE groups, framework groups or your own rule,
* Optional creation cooldown and cost, a name lock after deletion, and auto-expiring invites,
* Runs fully standalone, or alongside ESX, QB and QBOX,
* Logging to Discord, Fivemanage, Fivemerr, ox\_lib or a custom handler,
* A rich export API and server events so the rest of your server can build on gang data
