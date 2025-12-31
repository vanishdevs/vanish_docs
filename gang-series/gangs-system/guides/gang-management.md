# Gang Management

This guide explains how gang leaders can manage their gangs directly in-game using the built-in Gang Management Menu.

**Access Requirements:**

* Must be a gang leader (highest rank in your gang)
* Access at configured management locations via interaction marker or NPC

***

### 📋 Main Gang Management Menu Options

The management menu provides the following options:

#### 1. Add Members

* Invite new players to join your gang
* Enter the target player's server ID
* Player receives an invite notification
* Player must accept the invite to join
* New members start at rank 1

**Restrictions:**

* Target player must be online
* Target player cannot already be in a gang
* Target player cannot have a pending invite

***

#### 2. Manage Members

* View all current gang members (online only)
* Select a member to manage their rank or remove them
* Each member shows their name and current rank

**Available Actions per Member:**

* **Promote** - Increase their rank number
* **Demote** - Decrease their rank number
* **Kick** - Remove them from the gang immediately

**Promotion/Demotion Rules:**

* New rank must be between 1 and your gang's maximum rank
* Cannot promote members to the leadership rank
* Cannot kick or demote gang leaders
* Target player must be online

***

#### 3. Inventory Access

* Open your gang's shared stash/inventory
* Protected by a 4-digit passcode
* Passcode is set by server admins via the Gang Admin Menu

**Access Process:**

1. Select "Inventory Access" from the menu
2. Enter the gang's 4-digit passcode
3. If correct, the stash opens immediately
4. If incorrect or cancelled, access is denied with a notification

***

### 📍 Management Locations

Gang leaders access the management menu at configured locations:

#### Single Location Mode

* One fixed management point
* Optional NPC interaction
* Interaction via target system or marker press

#### Multiple Locations Mode

* Several configurable management spots
* Each location can have its own NPC
* Useful for territory-based setups

#### Interaction Methods

* **Target System** - Look at NPC or marker and use target key
* **Marker Interaction** - Press `E` when near the marker
* **NPCs** - Use scenario animations (e.g., clipboard, leaning)
* NPCs are invincible and cannot be moved

***

### 🔔 Notifications & Chat Messages

#### Player Notifications

* **Invite Sent** - Confirmation when you successfully invite someone
* **Access Denied** - When attempting to use features without permission
* **Invalid Input** - When entering invalid data (wrong passcode, invalid rank, etc.)
* **Player Offline** - When trying to manage an offline member
* **Player Already in Gang** - When inviting someone who's already in a gang

#### Chat Notifications

When enabled in config (`config.management.notifications.enabled`):

* New member joins the gang
* Member is kicked from the gang
* Member leaves the gang

***

### ⚙️ Permissions & Restrictions

#### Leader-Only Features

Only gang leaders (players with the highest rank) can:

* Access the Gang Management Menu
* Invite new members
* Promote/demote members
* Kick members

#### Protection Rules

* Leaders cannot be kicked from the gang
* Leaders cannot be demoted
* Cannot promote members to the leadership rank
* Cannot manage offline players (except viewing in admin menu)

#### Failed Actions

When an action fails, you'll receive a notification explaining why:

* "No permission" - Not a gang leader
* "Player not online" - Target player is offline
* "Player already in gang" - Cannot invite
* "Player has outstanding invite" - Already invited by someone
* Invalid passcode, rank, or input data

***

### 💡 Tips for Gang Leaders

1. **Member Invites** - Coordinate with potential members before sending invites
2. **Rank Structure** - Understand your gang's rank hierarchy before promoting/demoting
3. **Passcode Security** - Keep your gang's inventory passcode private
4. **Online Members** - You can only manage members who are currently online
5. **HUD Display** - Members see their gang info on the HUD (if enabled in config)
