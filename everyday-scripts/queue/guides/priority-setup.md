---
description: >-
  A practical setup path for priority sources, Discord roles, database priority,
  grace periods and the adaptive card.
---

# Priority Setup

The queue admits players by priority points when the server is full. A clean
setup starts with slot counting, then adds priority sources one at a time.

## Setup Order

1. Put `ensure vanish_queue` near the top of `server.cfg`.
2. Set `sv_maxclients` in `server.cfg`.
3. Import `sql/vanish_queue.sql` if `database.enabled = true`.
4. Set `serverName`, `alwaysShowQueue`, `minimumQueueTime` and `joinDelay`.
5. Decide your base priority in `priority.default`.
6. Add database priority for staff, VIP or temporary rewards.
7. Add Discord role priority if you want roles to affect the queue.
8. Tune grace priority for reconnects.
9. Customize the waiting card.

{% hint style="warning" %}
`sv_maxclients` is the real slot count. `maxPlayers` in the config is only a
fallback if that convar is missing.
{% endhint %}

## Priority Sources

Every player starts with `priority.default`. Other sources can add to it:

| Source | Config or command | Use |
| --- | --- | --- |
| Database | `/queue:add`, `/queue:remove`, exports | Staff-given priority, VIP packages, rewards. |
| Discord | `discord.roles` | Role-based priority from your Discord server. |
| Grace | `grace.enabled` | Temporary priority after a disconnect. |

With `priority.stackPriority = true`, all sources add together. With it set to
false, only the highest source counts.

## Database Priority

Database priority needs `database.enabled = true` and the SQL imported.

Useful admin commands:

| Command | Example | Result |
| --- | --- | --- |
| `/queue:add` | `/queue:add license:abc123 50 vip` | Adds 50 points under the `vip` category. |
| `/queue:remove` | `/queue:remove license:abc123 vip` | Removes that category. |
| `/queue:list` | `/queue:list` | Shows the current queue. |
| `/queue:clear` | `/queue:clear` | Clears the waiting queue. |

Use categories so you can remove one reason without wiping all priority from a
player.

## Discord Role Priority

Discord features need:

1. `discord.enabled = true`,
2. A bot token and guild ID in `config/config_discord.lua`,
3. The bot invited to your server,
4. Server Members Intent enabled for the bot,
5. Role IDs added under `discord.roles`.

Example:

```lua
roles = {
    ['123456789012345678'] = { priority = 100, label = 'Staff' },
    ['234567890123456789'] = { priority = 50,  label = 'VIP' },
}
```

Set `discord.requireMembership = true` only when everyone must be in the Discord
to connect. Use `discord.whitelist.enabled = true` only when one role should be
required for access.

## Grace Period

Grace priority helps players rejoin after a crash or restart.

```lua
grace = {
    enabled  = true,
    duration = 300,
    priority = 100,
}
```

Keep the duration long enough for a normal reconnect, but not so long that it
acts like permanent priority.

## What Players See

The adaptive card can show:

* Banner or title and icon,
* Current queue position,
* Queue list page,
* Priority breakdown page,
* Store and Discord buttons.

Edit `config/config_card.lua` before launch so players are not seeing placeholder
images or links. The priority page is especially useful when players ask why
someone passed them in line.
