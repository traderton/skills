# Changelog

All notable changes to this skills collection are documented here.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/).

## [Unreleased]

### Added

- Skills: ai4trade-trading-signals, crypto-trading, crypto-bot-management and crypto-risk-monitoring
- Initial commit

### Changed

- crypto-trading: ask-once setup flow, research-only until the user answers, and funding guidance using `get_funding_info`.
- crypto-trading: no ask-once question and no setup tool. The trading connection comes from `request_connection`; Traderton starts the profile in a test mode with a default capital; the agent states what is in effect using `get_account_summary`; and going live is started by the user, never the agent.
