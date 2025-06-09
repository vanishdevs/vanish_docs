---
description: >-
  A comprehensive, step-by-step installation guide detailing how to successfully
  set up my resource on your server.
---

# Installation

## Dependencies

The following dependencies listed below are required to the run this resource, without them, this resource will not work.

* ox\_inventory [https://github.com/overextended/ox\_inventory](https://github.com/overextended/ox\_inventory)
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox\_lib)

## Steps

1.  Add the following line in `ox_inventory > data > animations.lua` within the `anim` section

    ```lua
    ['switch'] = { dict = 'mp_arresting', clip = 'a_uncuff' },
    ```
2.  Copy and paste the following configuration into `ox_inventory > data > items.lua`

    ```lua
    ['switch'] = {
        label = 'Switch',
        weight = 200,
        stack = false,
        close = true,
        allowArmed = true,
        client = {
            anim = 'switch',
            usetime = 2500,
        },
        server = {
            export = 'vanish_switch.use_item_switch'
        }
    },
    ```
3. Download from keymaster and drag into your `resources` folder
4. Start the `vanish_switch`resource or ensure in the server.cfg
5. Ensure you have the dependencies needed to run the script
6. Configure the resource to your liking in the config.lua file
