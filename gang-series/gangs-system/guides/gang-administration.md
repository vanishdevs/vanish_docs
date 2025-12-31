# Gang Administration

This guide explains how server admins can manage gangs directly in-game using the built-in Gang Admin Menu.

**Access Methods:**

* Execute the `/gangadmin` command in-game
* Use the configurable keybind (set via `config.admin.keybind` in shared config)

**Permission Requirements:** Admin permissions are controlled through `config.admin.permissions` which supports:

* ACE permissions (`ace`)
* ESX groups (`group`)
* Custom permission functions

***

### 📋 Main Admin Menu Options

When opening the Gang Admin Menu, you'll see:

1. **Create Gang** - Opens dialog to create a new gang organization
2. **Gang List** - View and manage all existing gangs (each gang opens its Gang Management Menu)

***

### 🧩 Gang Management Menu

For each gang, you have access to the following options:

1. **Create Rank** - Add a new rank to this gang's hierarchy
2. **Manage Ranks** - Opens the Rank Management Menu for detailed rank control
3. **Set Passcode** - Update the gang's inventory/stash access code (4-digit PIN)
4. **Delete Gang** - Permanently remove the gang and all associated data
   * Removes all members from the gang
   * Deletes all gang ranks
   * Removes gang from database

***

### 🧱 Rank Management Menu

Ranks are numbered starting from **1** (lowest) up to the **leadership rank** (highest).

**Important:** The leadership rank number is defined when creating the gang and represents the highest authority level.

#### Within Each Gang's Rank Manager:

**For Each Rank:**

* **View Details** - See rank name, label, and position in hierarchy
* **Edit Rank** - Modify rank name and display label
* **Change Position** - Reorder rank in the hierarchy (change rank number)
* **Delete Rank** - Remove rank from gang
  * Members with this rank are automatically moved to rank 1
  * Quick deletion available via right-click or long-press

**Rank Constraints:**

* Rank numbers must be between 1 and the gang's leadership rank
* Each rank must have a unique number within the gang
* Rank names must start with a letter and be alphanumeric
* Leadership rank (highest number) cannot be assigned through promotion - it's reserved for gang leaders

***

### 🆕 Creating a Gang

When creating a new gang, you'll be prompted for:

1. **Gang Name** - Unique identifier (alphanumeric, must start with a letter, lowercase)
2. **Gang Label** - Display name shown to players
3. **Leadership Rank** - Highest rank number (defines gang hierarchy depth)
4. **Passcode** - 4-digit PIN for inventory/stash access

**Example:**

* Name: `ballas`
* Label: `The Ballas`
* Leadership Rank: `5` (creates ranks 1-5, where 5 is leader)
* Passcode: `1234`

***

### 🔧 Creating a Rank

When creating a new rank, you'll be prompted for:

1. **Gang** - Select which gang this rank belongs to
2. **Rank Name** - Internal identifier (alphanumeric, must start with a letter, lowercase)
3. **Rank Label** - Display name shown to players
4. **Rank Number** - Position in hierarchy (1 to leadership rank)

**Example:**

* Gang: `ballas`
* Name: `soldier`
* Label: `Soldier`
* Rank Number: `3`

**Requirements:**

* Rank number must be within the gang's range (1 to leadership rank)
* Rank name must be unique within the gang
* Rank number must not already exist for this gang

***

### ⚠️ Important Notes

* **Deleting a gang** removes ALL associated data including members and ranks
* **Deleting a rank** automatically demotes affected members to rank 1
* **Passcodes** must be exactly 4 digits
* **Leadership ranks** (highest rank number) have special privileges and cannot be promoted to
* All changes take effect immediately and update the cache automatically
* Member management (invite, kick, promote, demote) is handled by gang leaders through the in-game gang menu
