---
description: >-
  Setup for Weapon Editor, including oxmysql audit storage, supported inventory
  installers and the resource order to use.
---

# Installation

## Dependencies

* ox_lib [https://github.com/overextended/ox_lib](https://github.com/overextended/ox_lib)
* oxmysql

### Optional integrations

* ox_inventory, qb-inventory or qs-inventory if you want the installer and install-state checks to work with your inventory.
* ACE permissions or `shared/config_permissions.lua` rules for staff access.

## Steps

1. Download from keymaster and drag `vanish_weaponeditor` into your `resources` folder.
2. Make sure `ox_lib` and `oxmysql` start before `vanish_weaponeditor`.
3. Add `ensure vanish_weaponeditor` to your `server.cfg`.
4. Start the server or restart the resource.
5. Open the editor with `/weaponeditor`.
6. Configure access in `shared/config.lua` or enable the advanced rules in `shared/config_permissions.lua`.

The audit and backup tables are created automatically on first start from
`sql/vanish_weaponeditor.sql`, so there is no manual SQL import step.

{% hint style="warning" %}
Give editor access only to trusted staff. The resource can install weapons,
write inventory item files, edit weapon meta values and restore backups.
{% endhint %}
