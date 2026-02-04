---
description: >-
  A tidy, themeable shop system for FiveM that rotates inventory, respects
  custom currencies and permissions, and gives players a smooth NUI storefront
  backed by real-time stock control.
---

# 🛍️ Shops

## Features

* Multi-location shop modes with themed UI (General Store, Black Market) and distinct category icons/gradients per mode.
* Rotating inventory with per-item chance, guaranteed categories, min-per-category, and max-item caps; refreshes on restart or admin rotate.
* Config-driven items per mode: category, description, per-payment prices, limited/unlimited stock, and rotation chance.
* Flexible payments (money or item currencies) with mode restrictions and live balance retrieval before purchase.
* Server-side stock management for limited/unlimited items, plus queries/mutations via restock, rotate, and setstock commands.
* Client NUI storefront pulling fresh config/items/stock/balances; opens via command, keybind, or exports; manages focus cleanly.
* NPC or marker world interactions with configurable spawn distance, models, scenarios, markers, and target icons.
* Admin commands to restock all items, rotate inventory, and set specific item stock with permission checks.
* Webhook-ready logging (purchases, stock changes, rotations, admin actions) with Discord embed colors and toggles.
* Localization through ox\_lib locales and configurable item image base path (e.g., ox\_inventory) for consistent visuals.
* Exports on client (open/close/check UI) and server (get/set/add/restock stock) for easy integration with other resources.
* Integration-ready architecture using ox\_lib callbacks and oxmysql to validate purchases, deduct payments, and sync stock in real time.

## Preview

{% embed url="https://www.youtube.com/watch?v=oCm7TuduWjc" %}
