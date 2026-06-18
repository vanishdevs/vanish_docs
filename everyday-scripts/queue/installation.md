---
description: >-
  Setup for the queue, including the database table and the server settings it
  relies on to count slots correctly.
---

# Installation

## Dependencies

* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)
* oxmysql (only if you want database backed priority)
* OneSync enabled
* Server artifact build 6761 or newer

{% hint style="info" %}
The queue is framework independent. It works the same on ESX, QB, QBOX or a standalone server because it sits in front of the connection, before any framework loads the player.
{% endhint %}

## Steps

1. Download from keymaster and drag `vanish_queue` into your `resources` folder.
2. Import `sql/vanish_queue.sql` into your database. This is only needed if `database.enabled` is true in the config.
3. Add `ensure vanish_queue` to your `server.cfg`. Put it near the top, before your other resources, so it is ready as players connect.
4. Make sure `sv_maxclients` is set in your `server.cfg`. The queue uses it to know how many slots you have.
5. Open `config/config.lua` and set your server name, priority and Discord options.
6. Restart the server.

{% hint style="warning" %}
The queue reads your real slot count from `sv_maxclients`. The `maxPlayers` value in the config is only a fallback for when that convar is not set, so set `sv_maxclients` properly to avoid surprises.
{% endhint %}

## Discord setup

Discord features are optional and off by default. If you want role based priority or whitelisting:

1. Create a bot at [https://discord.com/developers/applications](https://discord.com/developers/applications) and enable the **Server Members Intent** on it.
2. Put the bot token and your guild (server) ID into `config/config_discord.lua`.
3. Invite the bot to your Discord server.
4. Set `discord.enabled = true` in `config/config.lua` and add your role IDs under `discord.roles`.

{% hint style="warning" %}
`config_discord.lua` holds your bot token. Keep it private and never commit it anywhere public.
{% endhint %}
