---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

## Configuration

The weapon editor reads two config files at startup:

* **`shared/config.lua`** — general settings: command, scanner, installer, UI, backups, audit, rate limiting.
* **`shared/config_permissions.lua`** — optional fine-grained capability rules. When disabled, the simple `admin.configure` list in `config.lua` is used instead.

{% hint style="warning" %}
Both files are reloaded only on resource restart. After editing, run `restart vanish_weaponeditor` in the server console.
{% endhint %}

### General

```lua
debug = true,
command = 'weaponeditor',
identifierPriority = {
    'license:', 'license2:', 'fivem:', 'steam:', 'discord:', 'xbl:', 'live:',
},
admin = {
    configure = { 'group.admin', 'group.god' },
},
```

| Setting              | Type    | Description                                                                                                              |
| -------------------- | ------- | ------------------------------------------------------------------------------------------------------------------------ |
| `debug`              | boolean | Prints scan results and errors to the server console. Disable in production.                                             |
| `command`            | string  | Chat command that opens the editor UI (e.g. `/weaponeditor`).                                                            |
| `identifierPriority` | array   | Order in which player identifiers are tried when writing audit logs. The first identifier present on the player is used. |
| `admin.configure`    | array   | ACE groups that can open the editor. Used only when `config_permissions.lua` has `enabled = false`.                      |

### Scanner

Controls how the editor discovers weapon `.meta` files across your server's resources.

```lua
scanner = {
    scanOnStartup    = true,
    batchSize        = 10,
    batchDelayMs     = 50,
    cacheResultsMs   = 30000,
    directories      = { '', 'data', 'meta', 'stream', 'weapons', 'components' },
    metaFiles        = { 'weapons.meta', 'weaponarchetypes.meta', 'weaponcomponents.meta', 'weaponanimations.meta', 'pedpersonality.meta' },
    excludeResources = {},
    customResourcePaths = {},
}
```

| Setting               | Type    | Default             | Description                                                                                               |
| --------------------- | ------- | ------------------- | --------------------------------------------------------------------------------------------------------- |
| `scanOnStartup`       | boolean | `true`              | Runs a full scan when the resource starts.                                                                |
| `batchSize`           | number  | `10`                | Resources processed per batch before yielding back to the server tick.                                    |
| `batchDelayMs`        | number  | `50`                | Delay between batches in ms. Increase on low-spec servers to reduce hitching.                             |
| `cacheResultsMs`      | number  | `30000`             | How long a scan result is cached before a rescan is allowed. Force-bypass via `ScanWeapons(true)`.        |
| `directories`         | array   | Six common dirs     | Flat directory paths to check inside each resource. `''` means the resource root.                         |
| `metaFiles`           | array   | Five standard files | Meta file names searched for in each directory above.                                                     |
| `excludeResources`    | array   | `{}`                | Resource names to skip entirely (e.g. base game packs you've already audited).                            |
| `customResourcePaths` | table   | `{}`                | Explicit subdirectory list per resource for nested layouts auto-detection can't reach. See example below. |

#### Custom resource paths

For packs with non-standard nested layouts (e.g. `data/pistol_a/weapons.meta` and `data/rifle_a/weapons.meta`):

```lua
customResourcePaths = {
    ['my_weapon_pack'] = { 'data/pistol_a', 'data/rifle_a' },
},
```

{% hint style="info" %}
Most packs are detected automatically via fxmanifest parsing and client script analysis. Only add an entry here when a rescan reports zero weapons for a pack you know contains them.
{% endhint %}

### Installer

Controls writes to your inventory resource (ox\_inventory, qb-inventory, or qs-inventory).

```lua
installer = {
    autoDetectComponents = true,
    preventDuplicates    = true,
}
```

| Setting                | Type    | Description                                                                                              |
| ---------------------- | ------- | -------------------------------------------------------------------------------------------------------- |
| `autoDetectComponents` | boolean | Automatically links and installs known components (clips, scopes, suppressors) when installing a weapon. |
| `preventDuplicates`    | boolean | Blocks an install if the weapon already exists in the inventory file. Recommended to keep on.            |

### Meta Editor

Controls the in-game `.meta` XML editor.

```lua
metaEditor = {
    enabled            = true,
    allowDirectWrite   = true,
    validateBeforeSave = true,
}
```

| Setting              | Type    | Description                                                                                                 |
| -------------------- | ------- | ----------------------------------------------------------------------------------------------------------- |
| `enabled`            | boolean | Master toggle for the editor. When `false`, the UI is read-only.                                            |
| `allowDirectWrite`   | boolean | Allows the editor to write back to the original `.meta` file. Disable for staging/review-only environments. |
| `validateBeforeSave` | boolean | Runs XML structure and required-field validation before saving. Strongly recommended to keep on.            |

### Backups

Automatic versioning whenever a meta file is modified.

```lua
backups = {
    enabled              = true,
    keepPerFile          = 20,
    allowRollback        = true,
    createBeforeRollback = true,
}
```

| Setting                | Type    | Description                                                                           |
| ---------------------- | ------- | ------------------------------------------------------------------------------------- |
| `enabled`              | boolean | Master toggle for the backup system.                                                  |
| `keepPerFile`          | number  | Max backup versions retained per file. Oldest are pruned automatically.               |
| `allowRollback`        | boolean | Exposes the rollback button in the UI.                                                |
| `createBeforeRollback` | boolean | Creates a safety backup of the current state before applying a rollback. Recommended. |

### Audit

Database-backed action log of every editor change.

```lua
audit = {
    enabled              = true,
    defaultPageSize      = 25,
    maxPageSize          = 100,
    maxEntries           = 10000,
    pruneBatchSize       = 500,
    pruneIntervalSeconds = 120,
}
```

| Setting                | Type    | Description                                                               |
| ---------------------- | ------- | ------------------------------------------------------------------------- |
| `enabled`              | boolean | Logs every editor action (scan, install, edit, rollback) to the database. |
| `defaultPageSize`      | number  | Entries per page in the audit log UI.                                     |
| `maxPageSize`          | number  | Hard cap on entries a client can request in a single query.               |
| `maxEntries`           | number  | Total entries kept in the database. Oldest are pruned when exceeded.      |
| `pruneBatchSize`       | number  | How many entries to delete in one prune cycle.                            |
| `pruneIntervalSeconds` | number  | How often the prune job runs.                                             |

### Rate Limiting

Per-player throttling on editor actions to prevent abuse.

```lua
rateLimit = {
    enabled   = true,
    scanMs    = 5000,
    installMs = 2000,
    editMs    = 1000,
    searchMs  = 1500,
}
```

| Setting     | Type    | Description                                           |
| ----------- | ------- | ----------------------------------------------------- |
| `enabled`   | boolean | Master toggle for rate limiting.                      |
| `scanMs`    | number  | Minimum interval between rescan requests, per player. |
| `installMs` | number  | Minimum interval between weapon/component installs.   |
| `editMs`    | number  | Minimum interval between meta file save operations.   |
| `searchMs`  | number  | Minimum interval between search/filter queries.       |

### UI

Visual and feature toggles for the React-based editor UI.

```lua
ui = {
    defaultTheme        = 'dark',
    showAuditLog        = true,
    allowBulkOperations = true,
    showWeaponPreview   = true,
    showComponentTree   = true,
    statBars            = true,
}
```

| Setting               | Type    | Description                                                                              |
| --------------------- | ------- | ---------------------------------------------------------------------------------------- |
| `defaultTheme`        | string  | Initial theme when the editor opens. `'dark'` or `'light'`. Users can toggle in-session. |
| `showAuditLog`        | boolean | Shows the audit log tab in the UI.                                                       |
| `allowBulkOperations` | boolean | Enables multi-select install/uninstall actions.                                          |
| `showWeaponPreview`   | boolean | Shows the in-world 3D weapon preview panel.                                              |
| `showComponentTree`   | boolean | Shows the linked-component attachment tree on the weapon detail view.                    |
| `statBars`            | boolean | Renders stat bars (damage, range, fire rate, accuracy) on the weapon detail header.      |

### Advanced Permissions

`shared/config_permissions.lua` provides a capability-based RBAC system that overrides the simple `admin.configure` list when enabled.

```lua
return {
    enabled = false,
    cacheMs = 2000,

    rules = {
        {
            priority = 100,
            match = { aces = { 'group.god' } },
            capabilities = { all = true },
        },
        {
            priority = 50,
            match = { aces = { 'group.admin' } },
            capabilities = {
                scanWeapons      = true,
                installWeapons   = true,
                uninstallWeapons = true,
                editMeta         = true,
                viewAudit        = true,
                manageBackups    = true,
                giveWeapon       = true,
            },
        },
        {
            priority = 10,
            match = { aces = { 'group.moderator' } },
            capabilities = {
                scanWeapons = true,
                viewAudit   = true,
            },
        },
    },
}
```

| Field     | Type    | Description                                                                                                                             |
| --------- | ------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `enabled` | boolean | When `true`, this file is used and `config.admin.configure` is ignored. When `false`, only the simple list applies.                     |
| `cacheMs` | number  | How long a player's resolved capabilities are cached server-side. Lower = faster permission changes, higher = less ACE lookup overhead. |
| `rules`   | array   | Ordered list of rules. Highest `priority` matches first. The first rule whose `match` block applies to the player wins.                 |

#### Capabilities

| Capability         | What it gates                                          |
| ------------------ | ------------------------------------------------------ |
| `all`              | Master flag — grants every capability below.           |
| `scanWeapons`      | Trigger a rescan from the dashboard.                   |
| `installWeapons`   | Install a weapon into the inventory resource.          |
| `uninstallWeapons` | Remove a weapon from the inventory resource.           |
| `editMeta`         | Edit weapon stats (single or bulk) and save raw XML.   |
| `viewAudit`        | View the audit log tab.                                |
| `manageBackups`    | View, restore, and delete file backups.                |
| `giveWeapon`       | Use the "Give to self" test button on a weapon detail. |

#### Match types

A rule's `match` block supports two keys. Both may be present on the same rule — if either matches, the rule applies.

```lua
match = { aces = { 'group.admin', 'group.god' } },                       -- ACE group / principal
match = { identifiers = { 'license:abc123...', 'steam:110000100000000' } }, -- Specific player identifiers
```

| Key           | Type  | Matches when…                                                                            |
| ------------- | ----- | ---------------------------------------------------------------------------------------- |
| `aces`        | array | Player passes `IsPlayerAceAllowed` for any listed ACE or principal (e.g. `group.admin`). |
| `identifiers` | array | Any of the player's identifiers (license, steam, fivem, discord, etc.) is in the list.   |

{% hint style="info" %}
Rules are evaluated highest-priority first. A player matching multiple rules gets the capabilities of the **highest-priority** match only — capabilities are not merged across rules.
{% endhint %}
