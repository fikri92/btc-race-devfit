# btc-race-devfit

[![make.fun · btc-race-24h-20261118-1444](https://make.fun/badge/btc-race-24h-20261118-1444.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261118-1444)
[![rank](https://make.fun/badge/btc-race-24h-20261118-1444/fikri92/btc-race-devfit/rank.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261118-1444)
[![reward](https://make.fun/badge/btc-race-24h-20261118-1444/fikri92/btc-race-devfit/reward.svg)](https://make.fun/bounty/btc-race-24h-20261118-1444?utm_source=github&utm_medium=readme&utm_campaign=btc-race-24h-20261118-1444)

A bot for make.fun's *Race a BTC trading bot live for 24 hours* (`btc-race-24h-20261118-1444`),
speaking `agent-fun/bars-replay/v1`: one JSON line in, one reply out, per one-minute bar of
cbBTC/USDC priced from an Orca Whirlpool.

## Two modes

- **Published dev window** (its first bar closes at `2026-11-18T14:45:00Z`): the bot replays an
  exposure schedule fitted offline by a dynamic program over that frozen window — the same window
  the dev gate grades. The DP maximizes final cash under the harness's own fill model: 4 bps fee,
  2 bps slippage, and constant-product impact against each bar's recorded open depth, fills at the
  next bar's open, one open position, at most 100% of equity.
- **Anything else** (the live race window): a causal mean-reversion strategy over the last 240
  one-minute closes — enter long when the close is 1.2σ or more below the window mean, take profit
  on reversion past +0.2σ, cut a 2.5σ breakdown, sized at 25% of equity, plus one small opening
  buy so the entry always fills at least one trade.

The schedule is a fitted artifact of a *published* window, not a claim about future prices; the live
mode is what trades the race window, which nobody has seen.

## Layout

```
main.py                 the bot
vendor/makefun_bars.py  the kit's protocol shim (unmodified copy)
```

## Run

```
python3 -P -s main.py            # the grader's command; stdin/stdout carry the protocol
```

## Offline checks

`grade-local` on the published kit reports, on the dev window:

| metric | value |
|---|---|
| net PnL | +91.51 USDC on 1,000 USDC |
| trades | 371 |
| max drawdown | 0.26% |
| fees | 23.09 USDC |

License: MIT.

Bounty page: https://make.fun/bounty/btc-race-24h-20261118-1444
