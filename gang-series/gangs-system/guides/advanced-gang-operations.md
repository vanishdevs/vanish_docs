---
description: >-
  How to use the newer gang tools without turning the docs into a config dump.
  Covers templates, recruitment, stash limits, health score and relationships.
---

# Advanced Gang Operations

This page is for servers that already have basic gangs working and want to use
the deeper management features cleanly.

## Create From Templates

Templates in `shared/config_creation.lua` prefill a new gang's rank ladder. The
default set includes street gang, motorcycle club, mafia family, cartel and an
empty custom option.

Use templates when you want staff to create gangs quickly without rebuilding the
same rank structure every time. A good setup flow is:

1. Pick the closest template when creating the gang.
2. Confirm the leadership rank matches the top rank in that template.
3. Rename any rank labels in the admin menu.
4. Adjust rank permissions after creation if your server uses custom leader or officer roles.

{% hint style="info" %}
`custom` is still available when you want to build every rank manually.
{% endhint %}

## Recruitment Flow

Recruitment gives gangs a cleaner path than staff manually adding every member.
Leaders can open or close applications, players can browse the recruitment board,
and leaders can accept or reject applicants.

Useful commands:

| Command | Who it is for | What it does |
| --- | --- | --- |
| `/gangboard` | Players | Opens the recruitment board. |
| `/gangopen` | Gang leaders | Opens applications for the player's gang. |
| `/gangclose` | Gang leaders | Closes applications. |
| `/gangapplications` | Gang leaders | Opens the application review list. |
| `/gangacceptapp` | Gang leaders | Accepts an application. |
| `/gangrejectapp` | Gang leaders | Rejects an application. |

Set `rateLimits.application` in `shared/config.lua` if players are spamming
applications. Reviewed applications are cleaned up by the retention settings.

## Stash Withdrawal Limits

The stash limit system lets you cap how much each rank can pull from the gang
stash over a rolling 24 hour window. Defaults live in `shared/config_stash.lua`
and are applied when a gang is created.

Each rank can have:

| Field | Meaning |
| --- | --- |
| `daily_value_cap` | Total value that rank can withdraw per day. `0` means unlimited. |
| `daily_item_cap` | Total item count that rank can withdraw per day. `0` means unlimited. |

Use stricter limits for new members and looser limits for trusted ranks. Leaders
can adjust per-gang values from the management UI after the defaults are seeded.

{% hint style="warning" %}
The item image path in `shared/config_stash.lua` needs to match your inventory.
If the stash page shows broken icons, fix `imagePath` before changing the limits.
{% endhint %}

## Health Score

Gang health score is a 0 to 100 admin signal. It is meant to show which gangs
are active and balanced, not to punish gangs automatically.

The score is based on:

* Active members seen recently,
* Rank balance,
* Recruitment activity,
* Recent activity log events,
* Whether the stash has any items.

Tune the weights in `shared/config_health.lua` to match what your server cares
about. A server focused on roster quality might give more weight to active
members and rank balance. A server focused on conflict and economy might weigh
recent activity and stash health higher.

Use `/ganghealth` for an admin check. Optional alerts can warn staff when a gang
crosses below the configured threshold.

## Relationships

Relationships let gangs mark each other as allied, neutral or rival. The public
export `GetGangRelationship(gangA, gangB)` returns that state for integrations
that want to react to diplomacy.

Use `/gangrelation` when you want leaders to update relationships in-game. Keep
the command permission in `shared/config.lua` aligned with your server policy:
open it to leaders if diplomacy is player-driven, or restrict it to staff if
relationships are part of admin-run storylines.

{% hint style="info" %}
Neutral is the fallback relationship. If no relationship has been set, other
resources should treat the gangs as neutral.
{% endhint %}
