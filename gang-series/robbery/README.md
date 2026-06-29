---
description: >-
  A full player-to-player robbery system with dispatch alerts, gang rules,
  cooldowns, a leaderboard and direct inventory access, built on ox_lib and
  ox_target.
icon: hand-holding
---

# Robbery

## Features

* Rob other players with a keybind, command or ox\_target interaction — choose any combination from config,
* Progress bar with animations for both the robber and victim during the hold-up,
* Victim raises hands with a dedicated toggle key; robbery can require hands up before it proceeds,
* Opens the victim's ox\_inventory on a successful robbery so the robber takes items directly from what they carry,
* Per-robber and per-victim independent cooldowns stop repeated targeting,
* Weapon requirement and vehicle restriction to keep interactions grounded,
* Item whitelist to protect specific items (IDs, licences, phones) from ever being stolen,
* Job blacklist preventing certain jobs from robbing or being robbed, with individual identifier bans,
* Gang integration via vanish\_gangs: require gang membership, block same-gang or allied-gang robberies,
* Dispatch bridge compatible with ps-dispatch, cd\_dispatch, qs-dispatch, rcore\_dispatch and linden\_outlawalert, with a standalone blip fallback for servers without a dispatch resource,
* Robbery leaderboard with player profiles, sortable metrics and pagination, opened by command or keybind,
* Admin commands to toggle the system server-wide, clear cooldowns for individual players and reset the leaderboard,
* Discord, FiveManage, FiveMerr and ox\_lib logging adapters with per-event opt-in flags,
* Works on ESX, QB and QBOX
