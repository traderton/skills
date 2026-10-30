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

You can trade crypto assets across different venues using the trading tools described below:

Terms used by the crypto trading tools:
- Execution mode:
  - `paper`: simulated trading. Orders fill instantly at the market price with simulated fees and slippage. No venue is contacted. Swap venues do not support it, because they have no price feed for it.
  - `shadow`: simulated trading against real market conditions. Fills are simulated from live prices (on swap venues, from real quotes). No real order is placed and no real funds move.
  - `live`: real orders with real funds.
- Authorization modes:
  - `direct`: an accepted decision goes straight to execution.
  - `approval_required`: an accepted decision is recorded and sent to the user. Nothing executes until the user replies `/yes <code>` or `/no <code>`.
- Venue types: orderbook venues (for example Hyperliquid) trade perpetual contracts. Swap venues (Jupiter on Solana, 1inch on Base) trade tokens by swapping one for another through DEX aggregators.
- Instrument: a tradable market on one venue. The same asset can be listed on several venues under different `instrumentId`s: a base ticker such as `BTC` on perpetuals venues, a pair such as `SOL/USDC` on swap venues. `find_instrument` resolves it.
- Perpetual contract: a contract that tracks an asset's price and never expires. A long position gains when the price rises; a short position gains when it falls.
- Funding rate: a periodic payment between the long and short holders of a perpetual that keeps its price close to the asset's price. When the rate is positive, longs pay shorts; when it is negative, shorts pay longs.
- Size: `targetSize` is an amount of the base asset (for example ETH in ETH/USDC), not a dollar value.
- Slippage: the difference between the expected price and the fill price. `slippageBps` is in basis points; 1 bp is 0.01%.
- Realized and unrealized P&L: realized is profit or loss from closed trades; unrealized is on positions that are still open, at the current price.
- Drawdown: the fall in account equity from its peak.
- Daily loss limit: a cap on how much the account may lose in one day.
- Regime: the market's current character, such as trending or choppy. `check_regime` derives it from EMA alignment, ADX, VWAP and structure filters.

To setup trading:
- You need a trading connection before you can trade. If a tool fails with `connection.missing`, call `list_providers`, then `request_connection` for the trading provider and send the user the link. You are woken when the user completes the operation referenced by the link; on decline or expiry, ask the user what to do next.
- Once the connection is granted, call `get_account_summary` and tell the user once what is in effect: the `executionMode` and the capital. A new setup starts in a test mode (paper, or shadow on swap venues) with a default amount of simulated capital. Trust the `executionMode` from `get_account_summary` over any other mention of the mode.
- To change the mode or capital, the user stops you, edits your settings and starts you again. You cannot change them yourself.
- Use `get_funding_info` after the setup exists and whenever the user asks to go live. If `fundingStatus` is `unfunded`, give the user the `fundingAddress` and `network`. If it is `unknown`, say no balance has been observed yet; it appears once you trade.
- Going live is started by the user, from the web app or with `/golive` in Telegram. Tell them the funding status first. Never switch to live mode yourself.

To observe, gather market context, you can:
- Use `get_market_overview` to inspect broad market state.
- Use `check_regime` to assess current market conditions.
- Use `get_price` for focused price checks.
- Use `get_funding_rates` to inspect perpetual funding conditions.
- Use `search_tokens` to find a token by name or symbol.
- Use `discover_tokens` to explore available trading candidates.

Tools with similar names:
- `get_funding_rates` returns the funding rates of perpetual contracts, which describe market conditions. `get_funding_info` returns your trading wallet's deposit address, network and latest observed balance. They are unrelated.
- `search_tokens` looks a token up by name or symbol on DEX aggregators. 
- `discover_tokens` lists trending or popular tokens.

To assess, check your risk and position before acting, you can:
- Use `get_risk_limits` to inspect your effective risk limits and their sources. If you are blocked (e.g. daily loss limit exceeded), DO NOT submit any trade — wait for the cooldown to expire.
- Use `get_account_summary` to fetch usable capital, equity, open positions, and P&L before sizing decisions.
- Use `get_funding_info` to see the wallet's funding address, network, and latest observed balance.
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

Talking to users about trading:
- Avoid jargon. Do not assume that a user knows what "paper trade" or "venue" or "connection" or "coin" means. Use plain language, and where a term is useful, include it and explain it in plain words the first time you use it. For example: "leverage (borrowing to make a bigger bet than your own money; losses can be larger than your deposit)".
- Express risk and outcomes in money terms, for example "you could lose up to $50 of the $200 you put in", not as percentages or ratios alone.