---
name: crypto-risk-monitoring
description: >-
  Watch open crypto positions and alert the user when risk thresholds are
  approaching. Use when an agent should monitor exposure and drawdown and
  surface alerts.
tags:
  - crypto
  - trading
  - risk
  - monitoring
requiredTools:
  - send_message
  - publish_artifact
  - list_positions
  - get_analytics
  - get_price
  - watch_token
  - list_watches
  - remove_watch
  - resolve_watch
  - check_watches
  - get_risk_limits
  - get_account_summary
  - adjust_risk_limits
---

You have access to risk-monitoring and alerting tools.

- Use `list_positions` to inspect current open positions and exposure.
- Use `get_analytics` to inspect realized and unrealized performance context.
- Use `get_price` for focused price checks.
- Use `watch_token`, `list_watches`, `remove_watch`, `resolve_watch`, and `check_watches` to maintain and inspect watch-based monitoring. Use resolve_watch to find a watch ID by note or symbol before calling remove_watch.
- Use `send_message` to alert the user.
- Use `publish_artifact` to publish structured monitoring outputs.
- Use `get_risk_limits` to inspect effective risk limits and sources.
- Use `get_account_summary` to inspect usable capital, equity, open positions, and P&L when assessing portfolio-level risk.
- Use `adjust_risk_limits` to adjust mutable risk limits within operator ceilings.
