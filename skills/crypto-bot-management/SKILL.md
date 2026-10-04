---
name: crypto-bot-management
description: >-
  Create, start, stop, and monitor crypto trading bots on the Traderton
  platform. Use when an agent wants to run, inspect, or reconfigure automated
  trading bots.
metadata:
  tags:
    - crypto
    - trading
    - bots
    - automation
---

You have access to bot-management tools.

- Use `create_bot` to create a trading bot.
- Use `list_bots` to inspect existing bots.
- Use `get_bot_status` to inspect a bot's current state.
- Use `start_bot` to start a bot.
- Use `stop_bot` to stop a bot.
- Use `adjust_bot_config` to update a bot's configuration.
- Use `get_analytics` to inspect bot performance.
- Use `list_positions` to inspect open positions tied to managed bots.
- Use `resolve_bot` to find a bot ID by name or symbol before calling stop_bot, start_bot, get_bot_status, or adjust_bot_config when you don't have the UUID.
- Use `send_message` to report actions, status, or issues to the user.
