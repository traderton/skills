---
name: crypto-trading
description: >-
  Submit trade decisions and inspect trading state across the Traderton
  venues. Use when an agent needs to observe the market, assess risk and
  positions, and act on crypto trade intents.
tags:
  - crypto
  - trading
  - perpetuals
  - spot
requiredTools:
  - get_market_overview
  - check_regime
  - get_price
  - get_funding_rates
  - search_tokens
  - discover_tokens
  - get_risk_limits
  - get_account_summary
  - get_analytics
  - list_positions
  - watch_token
  - list_watches
  - remove_watch
  - resolve_watch
  - check_watches
  - find_instrument
  - submit_decision
  - adjust_risk_limits
  - assess_strategy_preset
  - change_strategy_preset
---

You have access to trading tools, grouped by workflow phase.

To observe, gather market context, you can:
- Use `get_market_overview` to inspect broad market state.
- Use `check_regime` to assess current market conditions.
- Use `get_price` for focused price checks.
- Use `get_funding_rates` to inspect perpetual funding conditions.
- Use `search_tokens` to find a token by name or symbol.
- Use `discover_tokens` to explore available trading candidates.

To assess, check your risk and position before acting, you can:
- Use `get_risk_limits` to inspect your effective risk limits and their sources. If you are blocked (e.g. daily loss limit exceeded), DO NOT submit any trade — wait for the cooldown to expire.
- Use `get_account_summary` to fetch usable capital, equity, open positions, and P&L before sizing decisions.
- Use `get_analytics` to inspect recent trading outcomes and exposure.
- Use `list_positions` to inspect current open positions.
- Use `watch_token`, `list_watches`, `remove_watch`, `resolve_watch`, and `check_watches` to maintain and inspect watch-based monitoring. Use resolve_watch to find a watch ID by note or symbol before calling remove_watch.

To decide, you can:
- Use `find_instrument` to resolve an instrumentId by symbol, name, or pair before calling submit_decision. Filter by venue (e.g. venue="jupiter" for Solana, venue="hyperliquid" for perpetuals).
- Use `submit_decision` to submit a trade intent for a specific instrument. Only call this after completing the Observe and Assess phases above.
- Use `adjust_risk_limits` to adjust mutable (default-derived) risk limits within operator ceilings.
- Use `assess_strategy_preset` and `change_strategy_preset` to review and switch the active strategy preset when it suits the task.
