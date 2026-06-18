---
description: >-
  Setup for Gun Jam, including ox_lib, optional framework job whitelists and
  where to tune weapon chances.
---

# Installation

## Dependencies

* ox_lib [https://github.com/overextended/ox_lib](https://github.com/overextended/ox_lib)

### Optional

* ESX, QB or QBOX if you want job-based whitelisting through the included bridges.

## Steps

1. Download from keymaster and drag `vanish_gunjam` into your `resources` folder.
2. Add `ensure vanish_gunjam` to your `server.cfg`, after `ox_lib` and after your framework if you use job whitelists.
3. Open `shared/config.lua` and tune the global jam behavior.
4. Adjust `shared/config_weapons.lua` and `shared/config_weapongroups.lua` for your weapon balance.
5. Add immune jobs, identifiers or ACE groups in `shared/config_whitelisted.lua`.
6. Start, or restart, the resource.

{% hint style="info" %}
Without a supported framework running, the jamming logic still works. Job
whitelists need a framework bridge because the script has to read the player's
job.
{% endhint %}
