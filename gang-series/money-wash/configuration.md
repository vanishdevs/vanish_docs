---
description: The full config, with the wash locations and fees explained.
---

# Configuration

## shared/config.lua

```lua
return {
	-- Notification Settings
	NotificationTitle = 'Money Wash Notification', -- Title displayed for pawnshop notifications
    	NotificationType = 'ox_lib', -- Type of notification system: 'ox_lib' or 'custom', if custom write your logic in shared.lua
    	NotificationPosition = 'top', -- Position of the notification

		-- Text UI Settings
	textUIposition = 'right-center', -- Position of the text UI
    	textUIicon = 'fa-solid fa-money-bill', -- Icon used for text UI

	-- Progress Settings
	UseProgress = true, -- Enable or disable progress bar
	ProgressType = 'bar', -- Type of progress bar: 'bar' or 'circle'
	
	-- Locations of Money Wash & their general settings
	Locations = {
		-- @coords vec3(x, y, z) | @minWash number | @maxWash number | @tax number (0.0 - 1.0, 1.0 being no tax)
		[1] = { coords = vec3(895.4895, -179.2529, 74.7002), minWash = 1, maxWash = 10000, tax = 0.1 },
		-- [2] = { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
		-- [3] = { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
		-- [4] = { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
		-- [5] = { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
	},

	-- Marker Settings
	ViewDistance = 10,
	Marker = {
		type = 1,
		size = vec3(1, 1, 0.1),
		color = vec3(255,255,255)
	},

	-- Blip Settings
	Blip = true,
	BlipTitle = 'Money Wash',
	BlipStyle = {
		colour = 1,
		sprite = 500,
		display = 4,
		scale = 0.8
	},
}
```

## Wash locations

Each entry in `Locations` is one wash point. Add as many as you like.

| Field | What it is |
| --- | --- |
| `coords` | Where the wash point sits in the world. |
| `minWash` | The smallest amount a player can wash in one go. |
| `maxWash` | The largest amount a player can wash in one go. |
| `tax` | The fee taken on the wash, as a fraction from 0.0 to 1.0. |

### How the fee works

`tax` is the cut the wash takes. The player receives the amount minus that fraction.

* `tax = 0.0` means no fee. Wash $10,000 and get $10,000 clean.
* `tax = 0.1` means a 10% fee. Wash $10,000 and get $9,000 clean.
* `tax = 0.25` means a 25% fee. Wash $10,000 and get $7,500 clean.

{% hint style="warning" %}
The comment inside the config file reads "1.0 being no tax", but the script actually treats `tax` as the fee taken, so `0.0` is no fee and higher values take more. Use the table above as the source of truth.
{% endhint %}
