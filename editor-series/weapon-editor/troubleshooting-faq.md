---
description: >-
  Common errors and fixes. Each section is named after the exact message the
  editor shows, so you can search this page for the text in your toast/console.
---

# Troubleshooting / FAQ

{% hint style="info" %}
Most issues are resolved by clicking **Rescan** on the dashboard. The scanner caches results for 30 seconds by default (`scanner.cacheResultsMs`) and a manual rescan bypasses that cache.
{% endhint %}

### Scanner

#### My custom weapon pack isn't showing up after a rescan

Three things to check, in order:

1. **Enable `debug = true`** in `shared/config.lua` and restart the resource. The server console will print which directories and meta files the scanner found per resource. If your pack isn't listed at all, the resource probably isn't started.
2. **Check the pack's folder layout.** The scanner looks in these directories by default: root, `data/`, `meta/`, `stream/`, `weapons/`, `components/`. Anything deeper is invisible unless picked up via fxmanifest parsing.
3. **Add a `customResourcePaths` entry** for packs with non-standard nested layouts. See the Configuration page for the exact format.

#### "Could not isolate weapon block in meta file"

The editor reads the source `.meta` file at save time and looks for an `<Item type="CWeaponInfo">` block whose `<Name>` matches the weapon being edited. This error means that block was not found. Common causes:

* The scanner cached a weapon name but the file on disk has been renamed or edited externally. **Rescan** to fix.
* The pack uses a non-standard wrapper (e.g. inside `<!-- ... -->` comments, `<![CDATA[ ... ]]>` blocks, or a custom root tag instead of `<Item>`). Either edit via the raw XML tool or fix the file to standard layout.
* The scanner picked the wrong meta file as the source. Add the pack to `customResourcePaths` so it points at the correct subdirectory.
* The file is not UTF-8 (BOM-prefixed UTF-16, etc.). Re-save it as UTF-8 without BOM.

#### "Weapon not found: WEAPON\_XYZ"

The weapon isn't in the scanner cache. Either:

* A rescan hasn't run since the file was added — click **Rescan**.
* The weapon's resource is in `scanner.excludeResources`.
* The weapon fails the `IsPlayerWeapon` check (e.g. ped/animal weapons are filtered out by design).

#### "Could not read source file"

The scanner indexed the path, but `LoadResourceFile` returned `nil` at save time. Usually means the file was deleted or moved between scan and edit. Rescan and retry.

### Installer

#### "No inventory system detected"

The editor's installer supports **`ox_inventory`**, **`qb-inventory`**, and **`qs-inventory`**. It auto-detects whichever one is running. If you see this error:

* Confirm one of the three is started (`ensure ox_inventory` in your `server.cfg`).
* The inventory resource must be started **before** `vanish_weaponeditor`. If it isn't, restart the editor: `restart vanish_weaponeditor`.

#### "Weapon is already installed in inventory"

The weapon entry already exists in the inventory's items file. Either:

* Click **Uninstall** first if you want to reinstall.
* Set `installer.preventDuplicates = false` in `shared/config.lua` to allow overwrites (not recommended).

#### "Inventory system does not support file-based installation"

The detected inventory exposes runtime item APIs but not a writable items file. File-based install is currently supported on **`ox_inventory`** only. For other inventories, weapons must be added to their item registry manually.

#### "Could not find insertion point in items file"

The installer couldn't locate a `return {` or `Items = {` opening in the inventory's items file — usually because the file has been heavily customized or uses a non-standard format. Open the file and confirm it starts with the expected items table structure.

#### "Failed to write to inventory items file"

The file is read-only, the server process doesn't have write permission, or the disk is full. Check OS-level file permissions on the inventory resource folder.

#### Component install creates a duplicate entry

You ran install with `mode = 'create'` when you should have used `mode = 'auto'` or `'append'`. The default is `auto` — which appends to an existing component item if one exists, only creating a new entry if none does. Force-create is intentional.

### Meta Editor

#### "Meta editing is disabled in config"

`metaEditor.enabled` is `false` in `shared/config.lua`. Set it to `true` and restart.

#### "Failed to save modified file"

`SaveResourceFile` returned false. The original meta file is read-only or the server lacks write permission. Check OS file permissions.

#### Validation errors

If you see `"X must be a number"`, `"X minimum is N"`, `"X maximum is N"` — your input is outside the field's allowed range. Each field has a min/max defined in `MetaEditorService.GetFieldMap()`. To bypass validation set `metaEditor.validateBeforeSave = false` (not recommended — invalid values can crash the game).

### Backups & Rollback

#### "Rollback is disabled in config"

`backups.allowRollback` is `false` in `shared/config.lua`. Set it to `true` and restart.

#### "Backup not found"

The backup ID doesn't exist or has been pruned. Backups are capped per file by `backups.keepPerFile` (default 20) — oldest are deleted automatically.

#### "Failed to write rollback content"

Same root cause as **"Failed to save modified file"** above — file permissions or read-only flag on the target meta file.

### Audit Log

#### The audit log is empty

* Confirm `audit.enabled = true` in `shared/config.lua`.
* Confirm `oxmysql` is started **before** `vanish_weaponeditor`.
* Check the server console for `^2[WeaponEditor]^0 Database initialized successfully!` on startup. If you see `^1Could not load sql/vanish_weaponeditor.sql` or `^3Database initialization completed with errors`, the table didn't create. Verify `oxmysql` has database write access and your DB user has `CREATE TABLE` permission.

{% hint style="info" %}
The audit table is created automatically on first start via server/db.lua — no manual SQL import is needed.
{% endhint %}

#### Audit log isn't recording new entries

The DB connection dropped or the table is corrupted. Check the oxmysql console output for query errors. Restart `oxmysql`, then `vanish_weaponeditor`.

### Permissions / Access

#### The UI won't open / no chat command response

* Confirm your player is in one of the ACE groups listed in `config.admin.configure`.
* Verify the ACE group with `IsPlayerAceAllowed` in a separate test command, or check `server.cfg` for the `add_ace` / `add_principal` lines.
* If `config_permissions.lua` has `enabled = true`, the simple `admin.configure` list is **ignored** — your player must match a rule in `config_permissions.lua` instead.

#### A specific button is greyed out / "permission denied" toast

`config_permissions.lua` is enabled and your matched rule doesn't grant the required capability. See the capability table on the Configuration page for which button maps to which capability.

#### Permission changes don't take effect immediately

`config_permissions.cacheMs` caches a player's resolved capabilities. Default is 2000ms. Lower this for testing or wait two seconds after changing your player's groups.

### Performance

#### Server hitches every time the editor scans

Your server has a lot of resources. Tune the scanner:

* Lower `scanner.batchSize` (default 10) — smaller batches yield to the server tick more often.
* Raise `scanner.batchDelayMs` (default 50) — adds a longer pause between batches.
* Disable `scanner.scanOnStartup` and trigger scans manually only when needed.
* Add unused resources to `scanner.excludeResources` so they're skipped entirely.

#### Editor feels slow when typing in the stat editor

Rate limiting is throttling save requests. Increase `rateLimit.editMs` is the wrong fix — the limit is there for a reason. Instead, use the bulk-edit mode (multi-select fields and submit once) rather than saving on every keystroke.

### Still stuck?

Enable `debug = true` in `shared/config.lua`, restart the resource, reproduce the issue, and send the server console output. The debug log will show exactly which scanner branch ran, which inventory bridge loaded, and any save/permission failures with context.
