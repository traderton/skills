---
name: crypto-trading
description: >-
  Submit trade decisions and inspect trading state across the Traderton
  venues. Use when an agent needs to observe the market, assess risk and
  positions, and act on crypto trade intents.
metadata:
  tags:
    - crypto
    - trading
    - perpetuals
    - spot
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

To review and switch your strategy preset, you can:
- Use `list_strategy_presets` to see the available presets (key, name, description, strategy type, signal bias, behavior version).
- Use `assess_strategy_preset` to get computed data for one or more symbols: evidence (regime, volatility, candle window) and, per preset, a scorecard plus flags (`creatorAllowed`, `evidenceFresh`, `regimeMatch`, `signalsVsCurrent`). This is data only — no recommendation, no narrative, free.
- Use `change_strategy_preset` to switch your active preset, referencing an assessment artifact and giving a reason (shown on the review screen).

How to read the assessment data:
- `creatorAllowed` is false when the creator's policy forbids that preset — you cannot switch to it.
- `evidenceFresh` is false when the preset's scan was stale — do not switch to a preset without fresh evidence.
- `regimeMatch` is a deterministic fit between the preset's signal bias and the current regime (trend-following fits a trending market; mean-reverting fits a choppy/range market).
- `signalsVsCurrent` is the preset's signal count divided by your current preset's (null when your current preset has none). **More signals is NOT better fit** — a preset that fires many signals can still be a poor match for the regime or your risk posture.

Switch only when the data clearly shows a better fit, and always say why (your reason is recorded and shown on the review screen). Respect the assessment and preset-change cooldowns — if `assess_strategy_preset` reports a cooldown, wait for `nextEligibleAt` before retrying.
