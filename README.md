# Claude Trading Toolkit

The Robinhood and day-trading skills I set up in [Claude Code](https://claude.com/claude-code), what each one does, and how to get them.

**The short version:** Claude can look up prices and company data, check a Robinhood account, and place orders after you approve them. A free add-on pack adds eleven day-trading tools. None of it predicts the market.

> Not financial advice. Most people who day trade lose money.

## 1. Robinhood connection

Robinhood's trading connector, added in Claude Code through the `/mcp` menu and signed in with a normal Robinhood login. No password is stored in a file. Claude can only trade in the one account you designate for it.

| Area | What Claude can do |
| --- | --- |
| Account | Show account value, cash, buying power, positions, average cost and realized profit or loss. |
| Prices | Live quotes, price history, bid and ask, and technical indicators for stocks, funds, indexes and crypto. |
| Research | Company fundamentals, analyst ratings, earnings dates and results, financial statements and SEC filings. |
| Finding ideas | Build and run stock scanners, and read popular watchlists and politicians' disclosed trades. |
| Orders | Preview and place stock, fund, options and crypto orders, including dollar-amount (fractional) buys, and cancel open orders. |
| Tracking | Create watchlists and price alerts. |

- **It cannot move money.** Deposits and withdrawals are done in the Robinhood app.
- **It only acts when asked.** Claude is not watching the market between messages.
- **Check the approval setting.** Robinhood can require you to approve each order in the app. If that is off, an order executes as soon as Claude sends it.

## 2. Day Trader skill pack (11 skills)

A free, open-source pack by Jake Schincariol: [Jakeschincariol/day-trader-skill](https://github.com/Jakeschincariol/day-trader-skill). Each skill is a slash command. They read free public data and hand results to each other in order: find a setup, check the news, time the entry, plan the exit, test the odds, size it, check the risk, then show the order.

| Command | Job | What it does |
| --- | --- | --- |
| `/trader-scanner` | Chart Scanner | Scans thousands of US stocks for setups such as breakouts and oversold bounces. |
| `/trader-news` | News Monitor | Pulls new headlines and company filings for the stocks you watch. |
| `/trader-entry` | Entry Timer | Watches a setup bar by bar and says when it triggers. |
| `/trader-exit` | Exit Planner | Sets the stop-loss and profit target before you enter. |
| `/trader-timemachine` | Time Machine | Replays the trade 10,000 times against the stock's recent history to estimate the odds. |
| `/trader-budget` | Budget Manager | Sizes the trade to your account and your maximum loss per trade. |
| `/trader-risk` | Risk Manager | Blocks any order that breaks your limits (loss per trade, loss per day, trades per day). |
| `/trader-broker` | Order Executor | Shows the order ticket and sends it only after your yes. Practice account by default. |
| `/trader-alerts` | Price Alerts | Flags when a price, stop or target is hit, and when to be out by the close. |
| `/trader-analyst` | Earnings Analyst | Summarises a company's reported numbers from SEC filings. |
| `/trader-pelosi` | Pelosi Tracker | Reads the stock trades members of Congress must disclose. |

### Install

Paste this into Claude Code:

```
https://github.com/Jakeschincariol/day-trader-skill

install skill
```

- **Built for Alpaca, not Robinhood.** Its order step is written for an [Alpaca](https://alpaca.markets) practice (paper trading) account, which uses fake money against real prices.
- **Needs Python 3.8 or newer.** Nothing else to install, and no account or API key for the research tools.
- **Its price data is free and unofficial.** Yahoo Finance prices can lag. Check anything important against your broker.

## 3. Custom practice-trading bot

A small Python program written in the same session. It trades an Alpaca **practice** account on its own and refuses real-money keys.

| Part | What it does |
| --- | --- |
| Strategy | Buys when a short moving average crosses above a longer one on 5-minute price bars. |
| Exits | Every buy carries a stop-loss and a profit target held by the broker. Everything is sold before the market closes. |
| Safety limits | Maximum position size, maximum loss per trade, a daily loss limit that shuts it off, and a cap on trades per day. |
| Backtest | A second script replays the same rules on about 60 days of real prices. |

**Honest result:** on the 60 trading days from August 21 to November 12, 2026 the bot's rules lost money: 151 trades, 36% winners, about $99 lost on positions averaging $764. It has not been run with real money.

## 4. One to skip

[robinhood-agent-skill](https://github.com/uzairansaruzi/robinhood-agent-skill) is a read-only portfolio advisor. It works through an unofficial Robinhood connection, and its installer asks for your Robinhood email, password and two-factor secret and saves them in plain text. The official connection in section 1 does the same job without that.

## 5. Ground rules worth copying

- **Practice first.** Run any strategy on fake money for a few weeks before real money.
- **Approve every order yourself.** Have Claude show the order and wait for your yes.
- **Never paste passwords or API keys into a chat.** Keys go in a local file.
- **Read third-party skills before installing.** Ask Claude to review the code first.
- **Keep long-term money separate.** Do not day trade with the account that holds your long-term funds.
- **Only use money you can lose.** Most day traders lose money over time.
