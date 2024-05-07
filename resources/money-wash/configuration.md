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
	
	-- Locations of Money Wash & their general settings
	Locations = {
		{ coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
		-- { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
		-- { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
		-- { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
		-- { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
		-- { coords = vec3(1, 1, 1), minWash = 1, maxWash = 10000, tax = 0.8 },
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
	BlipTitle = 'MoneyWash',
	BlipStyle = {
		colour = 1,
		sprite = 500,
		scale = 0.8
	},
}
```
