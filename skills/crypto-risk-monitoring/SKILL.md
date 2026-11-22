---
name: crypto-risk-monitoring
description: "Watch open crypto positions and alert the user when risk thresholds are approaching. Use when an agent should monitor exposure and drawdown and surface alerts."
metadata:
  tags:
    - crypto
    - trading
    - risk monitoring
---

You have access to risk-monitoring and alerting tools.

- Use `list_positions` to inspect current open positions and exposure.
- Use `get_analytics` to inspect realized outcomes (trades, win rate, P&L, fees) over a recent window or, with `allTime`, the whole lifetime. It does not return unrealized P&L.
- Use `get_price` for focused price checks.
- Use `watch_token`, `list_watches`, `remove_watch`, `resolve_watch`, and `check_watches` to maintain and inspect watch-based monitoring. Use resolve_watch to find a watch ID by note or symbol before calling remove_watch.
- Use `send_message` to alert the user.
- Use `publish_artifact` to publish structured monitoring outputs.
- Use `get_risk_limits` to inspect effective risk limits and sources.
- Use `get_account_summary` to assess portfolio-level risk: `equity`, `availableCapital`, `unrealizedPnlUsd`, realized P&L, `drawdown` and open positions with marks. `initialCapital` is the configured allocation, not current funds. Figures exclude funding payments and cover your own direct positions only. Check `asOf` for freshness.
- Use `adjust_risk_limits` to adjust mutable risk limits within operator ceilings.
