---
description: >-
  How gang leaders challenge each other, how wagers are funded, how wars score,
  and what admins can do when a war needs intervention.
---

# Running Gang Wars

Gang Phone turns gang conflict into a challenge flow with wagers, kill targets,
war HUD, markers and leaderboard stats.

## Leader Flow

1. A player opens the phone with the configured item, command or keybind.
2. The player chooses a gang to challenge. If `showOnlineGangsOnly = true`, only gangs with online members appear.
3. The challenger sets a kill target within `minKillTarget` and `maxKillTarget`.
4. The challenger sets a wager within `minWager` and `maxWager`.
5. The challenge is sent and stays pending until accepted, declined or expired.
6. A rank with `acceptWar = true` accepts the challenge.
7. Both gangs fight until one reaches the kill target, the timer runs out, or an admin force ends it.
8. Rewards are distributed based on the configured reward mode.

`maxWarsPerGang` keeps one gang from stacking too many fights. The server-wide
cap is `maxSimultaneousWars`.

{% hint style="info" %}
If `winBy2.enabled = true`, reaching the kill target is not enough unless the
gang also has the configured lead.
{% endhint %}

## Wagers and Payouts

Wagers can come from the leader, the gang stash, or either source depending on
`war.wagerFunding.mode`.

| Mode | How it behaves |
| --- | --- |
| `leader` | The leader pays from personal cash or bank. |
| `stash` | The gang stash pays using `stashItem`. |
| `leader_or_stash` | Either source can fund the war. `preferStash` controls the first attempt. |

When a gang wins, `rewards.winnerMultiplier` decides the payout. With the default
`2.0`, a `$10,000` wager pays `$20,000` to the winner.

Reward delivery is controlled by `rewards.distributionMode`:

* `leader` pays the gang leader,
* `members` splits the payout among online members,
* `stash` deposits to the gang stash and falls back to leader if stash delivery is unavailable.

Optional bonuses can reward flawless wins and comeback wins.

## Kill Rules

Kills are counted only when they pass the configured rules:

* Killer and victim must be close enough for `killDetection.distance`.
* The downed grace period stops the same downed player from being counted twice.
* Crutch behavior follows the `killDetection.crutch` settings when wasabi_crutch is running.
* If war zones are enabled, kills only count inside configured war zones.

Use zones when you want wars in arenas or agreed fight areas. Leave zones off
when wars should count anywhere on the map.

## Admin Controls

| Command | Use |
| --- | --- |
| `/listwars` | Lists active wars and their IDs. |
| `/endwar <warId>` | Force ends a war by ID. |

Use `/listwars` first when you need the right war ID. Force ending a war is the
same action exposed by the `ForceEndWar` export.

{% hint style="warning" %}
Make sure `rankPermissions` grants `challengeWar` and `acceptWar` only to ranks
you trust. Otherwise lower ranks may be able to start expensive fights.
{% endhint %}
