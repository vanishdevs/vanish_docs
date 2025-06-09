# Gang Administration

This guide explains how server admins can manage gangs directly in-game using the built-in **Gang Admin Menu**.&#x20;

The admin menu is opened via the `gangadmin` command or a configurable keybind (`config.GangAdminKeybind`).

***

## 📋Main Admin Menu Options

| **Action**             | **Description**                                                               |
| ---------------------- | ----------------------------------------------------------------------------- |
| **➕ Create Gang**      | Opens a prompt to create a new gang using `/creategang`.                      |
| **⚙️ Manage Gangs**    | Browse all existing gangs and manage their data.                              |
| **👤 Set Player Gang** | Set a player to a gang and assign a rank using `/setgang [id] [gang] [rank]`. |
| **🗑️ Remove Player**  | Remove a player from their gang using `/removefromgang [id]`.                 |

***

## 🧩Gang Management Menu

For each gang, you will have access to the following options:

| **Action**                    | **Description**                                                         |
| ----------------------------- | ----------------------------------------------------------------------- |
| **🗑️ Delete Gang**           | Deletes the gang using `/deletegang [name]`.                            |
| **👥 View Members**           | Displays a list of current members.                                     |
| **🔐 Set Inventory Passcode** | Set the gang's inventory passcode via `/setpasscode [name] [passcode]`. |
| **⭐ Manage Ranks**            | Open rank management for the selected gang.                             |

***

## 🧱Rank Management Menu

{% hint style="danger" %}
Ranks start from **0** as the first rank and increment up to the maximum number defined as the leadership rank.
{% endhint %}

Within each gang’s rank manager:

| **Action**                     | **Description**                                        |
| ------------------------------ | ------------------------------------------------------ |
| **➕ Create Rank**              | Uses `/createrank` to define a new rank.               |
| **✏️ Edit Rank Label**         | Update the display label using `/updateranklabel`.     |
| **🔢 Edit Rank Order (Level)** | Change the numerical level using `/updaterankranking`. |
| **🗑️ Delete Rank**            | Remove the rank with `/deleterank [gang] [level]`.     |

Each rank is listed with a right-click (or long-press) action for quick deletion as well.
