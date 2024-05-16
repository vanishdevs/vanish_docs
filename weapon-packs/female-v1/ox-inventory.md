---
description: >-
  Only refer to this page if you are using ox inventory as your inventory
  resource. If you are using any other inventory refer to those sub-pages. If
  you notice any errors here, open a ticket.
---

# Ox Inventory

* Insert the weapons into your `weapons.lua`

```lua
-- vanishdev's Female Weapons V1
['WEAPON_FDESERTEAGLE'] = {
	label = 'Female Desert Eagle',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-9',
},
['WEAPON_FDRACOSMG'] = {
	label = 'Female Draco',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-rifle2',
},
['WEAPON_FFNFAL'] = {
	label = 'Female FNFAL',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-rifle2',
},
['WEAPON_FGLOCK18'] = {
	label = 'Female Glock 18',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-9',
},
['WEAPON_FKELTEC'] = {
	label = 'Female Keltec',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-9',
},
['WEAPON_FKELTECSUB'] = {
	label = 'Female Keltec Sub',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-9',
},
['WEAPON_FMLOK'] = {
	label = 'Female M-LOK',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-rifle',
},
['WEAPON_FNIGHTMARE'] = {
	label = 'Female Nightmare',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-rifle',
},
['WEAPON_FUSP'] = {
	label = 'Female USP',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-9',
},
['WEAPON_FUZI'] = {
	label = 'Female UZI',
	weight = 4500,
	durability = 0.6,
	ammoname = 'ammo-9',
},
```

* Insert/Replace this next snippet at the bottom of the `weapons.lua` file in order for attachments to work, if you don't want this, just skep this step

```lua
['at_clip_extended_pistol'] = {
	label = 'Extended Pistol Clip',
	type = 'magazine',
	weight = 280,
	client = {
		component = {
			`COMPONENT_APPISTOL_CLIP_02`,
			`COMPONENT_CERAMICPISTOL_CLIP_02`,
			`COMPONENT_COMBATPISTOL_CLIP_02`,
			`COMPONENT_HEAVYPISTOL_CLIP_02`,
			`COMPONENT_PISTOL_CLIP_02`,
			`COMPONENT_PISTOL_MK2_CLIP_02`,
			`COMPONENT_PISTOL50_CLIP_02`,
			`COMPONENT_SNSPISTOL_CLIP_02`,
			`COMPONENT_SNSPISTOL_MK2_CLIP_02`,
			`COMPONENT_VINTAGEPISTOL_CLIP_02`,
			`COMPONENT_TECPISTOL_CLIP_02`,
			 
			`COMPONENT_FUSP_CLIP_02`, -- vanishdev female v1
			`COMPONENT_FGLOCK18_CLIP_02`, -- vanishdev female v1
		},
		usetime = 2500
	}
},

['at_clip_extended_rifle'] = {
	label = 'Extended Rifle Clip',
	type = 'magazine',
	weight = 280,
	client = {
		component = {
			`COMPONENT_ADVANCEDRIFLE_CLIP_02`,
			`COMPONENT_ASSAULTRIFLE_CLIP_02`,
			`COMPONENT_ASSAULTRIFLE_MK2_CLIP_02`,
			`COMPONENT_BULLPUPRIFLE_CLIP_02`,
			`COMPONENT_BULLPUPRIFLE_MK2_CLIP_02`,
			`COMPONENT_CARBINERIFLE_CLIP_02`,
			`COMPONENT_CARBINERIFLE_MK2_CLIP_02`,
			`COMPONENT_COMPACTRIFLE_CLIP_02`,
			`COMPONENT_HEAVYRIFLE_CLIP_02`,
			`COMPONENT_MILITARYRIFLE_CLIP_02`,
			`COMPONENT_SPECIALCARBINE_CLIP_02`,
			`COMPONENT_SPECIALCARBINE_MK2_CLIP_02`,
			`COMPONENT_TACTICALRIFLE_CLIP_02`,
			`COMPONENT_CARBINERIFLE_BOXMAG`,
			
			`COMPONENT_FMLOK_CLIP_02`, -- vanishdev female v1
			`COMPONENT_FKELTEC_CLIP_02`, -- vanishdev female v1
		},
		usetime = 2500
	}
},

['at_clip_extended_smg'] = {
	label = 'Extended SMG Clip',
	type = 'magazine',
	weight = 280,
	client = {
		component = {
			`COMPONENT_ASSAULTSMG_CLIP_02`,
			`COMPONENT_COMBATPDW_CLIP_02`,
			`COMPONENT_MACHINEPISTOL_CLIP_02`,
			`COMPONENT_MICROSMG_CLIP_02`,
			`COMPONENT_MINISMG_CLIP_02`,
			`COMPONENT_SMG_CLIP_02`,
			`COMPONENT_SMG_MK2_CLIP_02`,
			
			`COMPONENT_FUZI_CLIP_02`, -- vanishdev female v1
			`COMPONENT_FKELTECSUB_CLIP_02`, -- vanishdev female v1
		},
		usetime = 2500
	}
},
```
