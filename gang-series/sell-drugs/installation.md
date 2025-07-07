---
description: >-
  A comprehensive, step-by-step installation guide detailing how to successfully
  set up my resource on your server.
---

# Installation

## Dependencies

The following dependencies listed below are required to the run this resource, without them, some features may not work, but otherwise, the script can be standalone

* ESX, QB, QBOX (editable bridge for more compatibility)
* wasabi\_ambulance or esx\_ambulancejob (editable bridge for more compatibility)
* cd\_dispatch, linden\_outlawalert, ps-dispatch or qs-dispatch (editable bridge for more compatibility)
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)

## Steps

1. Download from keymaster and drag into your `resources` folder
2. Run the `ranks.sql` file in your database, making sure the labelled lower rank is what you want it to be, matching the lowest rank in the config.
3. Start the `vanish_selldrugs` resource or ensure in the server.cfg
4. Ensure you have the dependencies needed to run the script
5. Configure the resource to your liking
