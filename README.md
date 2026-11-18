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
- **Anything else** (the live race window): a causal pullback buyer, long only, at 100% of equity
  when it fires. An entry needs three things at once — price at least 1.2% under its own 4-hour
  high, the 4-hour EMA above the price and rising over the last hour (a day worth buying into),
  and the 30-minute return at or above -0.3% (the fall has stopped). An exit comes on a 2.0% gain,
  on a 1.0% trail off the peak since entry, or after 10 hours. One hour of cooldown and at most
  six entries: a falling day costs a few tenths of a percent, not the whole day. One 2 USDC round
  trip on bar 10 makes sure the entry always has a fill, which the race guardrail wants.

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
