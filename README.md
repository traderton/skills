# skills

Traderton-owned crypto skill instructions, published for herobids'
external-backend resolution (herobids Step 10 / D11). Each skill is a portable
[Agent Skill](https://skills.sh) that describes the trading expertise and the
tools an agent uses within it. The instructions are backend-owned: herobids
resolves these refs (`traderton/skills/<skill>`) against its approved external
backend and exposes tools from the backend's signed descriptor.

## Available skills

| Skill | What it does |
|-------|--------------|
| [crypto-trading](skills/crypto-trading/SKILL.md) | Submit trade decisions and inspect trading state. |
| [crypto-bot-management](skills/crypto-bot-management/SKILL.md) | Create, start, stop, and monitor trading bots. |
| [crypto-risk-monitoring](skills/crypto-risk-monitoring/SKILL.md) | Watch open positions and alert the user when risk thresholds are approaching. |
