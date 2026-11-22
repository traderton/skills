---
name: crypto-ict-swing-trading
description: "An ICT-style swing trading approach for crypto: a daily EMA bias with hourly optimal trade entries (OTE), stop placement, scaling out and trade management. Use when an agent is asked to swing trade with this method, in rising or falling markets."
metadata:
  tags:
    - crypto
    - trading
    - swing trading
    - ict
    - ema
---

You can use the swing trading approach below to decide when to open a position, where to place a stop, and how to take profits. It is a discretionary method, not a guarantee of profit. It works alongside the `crypto-trading` skill: use the tools described there to observe, assess your risk and submit decisions. Your risk limits and position sizing come from `get_risk_limits` and `get_account_summary`; this approach does not set them.

Terms used by this approach:
- EMA: exponential moving average, a smoothed price line that reacts faster than a simple average. This approach uses the 10 EMA and the 20 EMA.
- Crossover: the 10 EMA moving from one side of the 20 EMA to the other.
- Stacking: the 10 EMA moving further away from the 20 EMA in the direction of the trend.
- Consolidation: price moving sideways in a range, with flat EMAs.
- Swing high / swing low: a local peak or trough in price.
- Impulse: a strong, one-directional move with large candles and little pullback.
- Retracement: a pullback against the last impulse.
- OTE (optimal trade entry): the 62%-79% zone of a Fibonacci retracement, drawn across the last impulse.
- Rejection: a candle or sequence of candles showing price tried to continue against the trade direction and failed, for example a long wick or a close back inside the zone.
- Liquidity high / low: a price level where many stop orders likely sit, such as a prior swing high or low, or equal highs or lows.
- Premium / discount: the upper / lower half of a range. Sellers look for entries in premium; buyers look for entries in discount.

Data this approach relies on:
- Daily and 1-hour candles for the instrument, and the 10 EMA and 20 EMA calculated on them.
- Swing highs and lows, to draw the Fibonacci retracement.
- Use the market tools described in `crypto-trading` (for example `check_regime` and `get_price`) and any other candle or indicator data available to you. If you cannot get candle data for a timeframe, say which input is missing and do not estimate the EMAs. Without the data, treat the setup as not confirmed.

How this approach reads the market:
- The daily chart sets the bias. The 1-hour chart times the entry.
- The bias is bullish from a bullish daily crossover until the 10 EMA crosses back below the 20 EMA. In that period it considers only long trades.
- The bias is bearish from a bearish daily crossover until the 10 EMA crosses back above the 20 EMA. In that period it considers only short trades.
- With no active bias, such as flat daily EMAs or a choppy range, it has no setup. Waiting is a valid outcome.

Daily bias, bullish:
- The market was consolidating, then price broke a significant swing high and momentum turned bullish.
- The 10 EMA crossed above the 20 EMA, both EMAs point upward, and the 10 EMA is stacking away from the 20 EMA.
- Price stays above both EMAs.

Daily bias, bearish (the mirror image):
- The market was consolidating, then price broke a significant swing low and momentum turned bearish.
- The 10 EMA crossed below the 20 EMA, both EMAs point downward, and the 10 EMA is stacking away from the 20 EMA.
- Price stays below both EMAs.

1-hour confirmation:
- Long: price is above the 20 EMA, has made an impulsive bullish move, and has created a swing high.
- Short: price is below the 20 EMA, has made an impulsive selloff, and has created a swing low.

Optimal trade entry (OTE):
- Long: draw the Fibonacci retracement from the swing low to the swing high. Wait for price to retrace into the 62%-79% zone. The retracement ideally reaches a previous swing low area, an imbalance (a gap left by a fast move), body support, or the discount half of the range.
- Short: draw the Fibonacci retracement from the swing high to the swing low. Wait for price to retrace into the 62%-79% zone, in the premium half of the range.
- A shallow retracement that does not reach the OTE zone is not a setup. Wait for a deeper one or skip the trade.

1-hour EMA filter:
- Long: price may briefly dip below the 20 EMA. The 10 EMA must stay above the 20 EMA. If the 10 EMA crosses below the 20 EMA, the long setup is cancelled.
- Short: price may briefly spike above the 20 EMA. The 10 EMA must stay below the 20 EMA. If the 10 EMA crosses above the 20 EMA, the short setup is cancelled.

Entry trigger. All of these hold:
- Price is in the OTE zone.
- A rejection appears in the trade direction (bullish for a long, bearish for a short).
- The 1-hour EMAs are still stacked in the trade direction.
- The daily bias is still active.

Stop placement:
- Long: place the stop below the swing low, the liquidity low, or the retracement low.
- Short: place the stop above the swing high, the liquidity high, or the retracement high.
- Size the position so that a loss at the stop stays within your risk limits. If the stop is so far away that no reasonable size fits those limits, skip the trade.

Profit taking:
- Scale out in parts. Take the first partial at the prior highs (long) or prior lows (short), equal highs or lows, a symmetrical price swing (a move equal in size to the previous swing), or a measured-move target.
- Keep a smaller runner for an extended move in the trade direction, such as a daily expansion or a continuation of the trend.

Managing an open trade:
- During consolidation, keep the stop wider and avoid trailing it tightly.
- After an expansion, trail the stop behind higher swing lows (long) or lower swing highs (short).
- Avoid trailing aggressively early in the trade.

Higher-probability conditions:
- The daily chart recently left a consolidation.
- The EMAs show strong stacking.
- Price expanded impulsively away from the EMAs.
- The 1-hour retracement reaches the OTE zone and respects the EMA structure.
- The market shows clear directional intent.

Conditions that weaken the setup, where staying out is reasonable:
- The daily EMAs are flat, or the market is choppy or consolidating.
- Price is on the wrong side of the 20 EMA for the bias.
- The 10 EMA crosses against the bias.
- The retracement is shallow and does not reach the OTE zone.
- There was no impulsive move before the retracement.

Summary of the flow:
1. Check the daily 10 EMA against the 20 EMA to find the bias.
2. Confirm the market broke out of consolidation in that direction.
3. Move to the 1-hour chart and wait for an impulse in the bias direction.
4. Draw the Fibonacci retracement and wait for price to reach the OTE zone.
5. Enter on a rejection, with a stop beyond the swing.
6. Take partial profits at the prior highs or lows.
7. Keep a runner and manage the stop as described above.

Talking to users about this approach:
- Say which bias is active and why in plain language, for example "the daily trend is up, so I am only looking for buying opportunities".
- When you skip a setup, say which condition was missing.
- Express risk in money terms, for example "if the stop is hit you would lose about $30 of the $500 in this trade".
