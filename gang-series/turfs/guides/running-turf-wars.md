---
description: >-
  A start-to-finish walkthrough for turf wars, live zone editing and the three
  reward modes.
---

# Running Turf Wars

This guide explains what players experience, what admins configure, and how to
choose the reward mode for each turf.

## Player Flow

1. A gang member goes to a zone's start point.
2. The player presses the configured interaction key, `E` by default.
3. The script checks global cooldown, zone cooldown, schedule, active turf caps and the gang's online member count.
4. If the start is allowed, the capture area becomes active.
5. Gang members fight inside the zone until the capture timer finishes or an admin ends it.
6. The winning gang receives the configured rewards.
7. Dead players in the zone are handled by the respawn settings.

The two biggest tuning knobs are `captureTime` on each zone and
`global.minStartingMembers`. Short capture times create quick street fights.
Longer times make the zone easier to contest and defend.

{% hint style="info" %}
If `global.singleActive = true`, only one turf can run anywhere at a time. If it
is false, multiple zones can be active as long as each zone passes its own checks.
{% endhint %}

## Building Zones Live

Use `/turfeditor` to build or adjust zones in-game. It is the safest way to line
up start points, capture areas, blips, peds and dropoff locations without
guessing coordinates by hand.

A practical build pass looks like this:

1. Open `/turfeditor` with an admin account.
2. Create the zone and give it a clear label.
3. Place the start point somewhere players can approach without standing inside the whole capture area.
4. Set the center area large enough for fighting but not so large that players can hide across several blocks.
5. Add rewards and test the item names against your inventory.
6. If using dropoff rewards, place the dropoff where the winning gang can collect without blocking the next fight.
7. Save, then start the zone with `/turfstart` for a controlled test.

The editor limits are set in `editor.maxLabelLength`, `editor.maxRewards` and
`editor.maxPeds`.

## Reward Modes

`rewards.distributionMode` sets the default for every zone. A zone can override
it when one turf needs a different feel.

| Mode | What players feel | Good for |
| --- | --- | --- |
| `inventory` | Rewards land straight in the winning gang's stash. | Passive territory income and low-friction payouts. |
| `players` | Winning members in the zone receive items directly. | Small fights where everyone present should get paid. |
| `dropoff` | A temporary stash appears at the zone's dropoff point. | Hot zones where winners still need to secure the payout. |

For `dropoff`, decide whether the pickup should be a marker, ped or prop, then
choose `key` or `target` interaction. Keep `restrictToWinner = true` unless you
want other gangs to steal the reward after the fight.

{% hint style="warning" %}
Reward item names must exist in your inventory. If a zone is not paying out,
check the item names before changing capture or cooldown settings.
{% endhint %}

## Admin Controls

Use these commands during setup and live events:

| Command | Use it when |
| --- | --- |
| `/turfs` | You want the admin panel. |
| `/turfstart` | You want to force start a configured zone. |
| `/turfsend` | You need to end an active turf cleanly. |
| `/turfextend` | You want to add time to a running turf. |
| `/setturfganglogo` | You want a gang logo shown in turf UI and leaderboard views. |

Player-facing commands are `/turfleaderboard` and `/turfsettings`. The actual
leaderboard command name comes from `leaderboard.commandName`.
