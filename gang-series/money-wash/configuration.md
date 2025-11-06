---
description: >-
  The configuration options are listed below, with no additional explanations
  provided on this page as the code comments thoroughly explain each option.
---

# Configuration

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
