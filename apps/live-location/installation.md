---
description: >-
  How to install the Live Location app, including the database table and the
  lb-phone setup. The app adds itself to the phone automatically.
---

# Installation

## Dependencies

* lb-phone
* ox\_lib [https://github.com/overextended/ox\_lib](https://github.com/overextended/ox_lib)
* oxmysql

{% hint style="info" %}
The app reads phone numbers and contacts straight from lb-phone, so it works on ESX, QB and QBOX without any extra framework setup.
{% endhint %}

## Steps

1. Download from keymaster and drag `vanish_livelocation` into your `resources` folder.
2. Import `sql/vanish_livelocation.sql` into your database. This creates the table that stores share permissions between players.
3. Add `ensure vanish_livelocation` to your `server.cfg`, after `lb-phone` and `oxmysql`.
4. Start, or restart, the resource.

That is it. The app registers itself with lb-phone on startup and appears on the phone automatically. If you restart lb-phone, the app re-adds itself.

## Renaming the app

The app's name, icon and description shown on the phone all come from the `app` block in `shared/config.lua`. Change them there if you want it to read as something other than "Find My Contacts".

{% hint style="warning" %}
The app is added as a default app, so it shows up on every player's phone. If you would rather players install it from the lb-phone app store, set `defaultApp` to false in the lb-phone bridge before you go live.
{% endhint %}
