# gta6-ccu-forecast

[![make.fun · gta6-twitch-ccu-forecast-01](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/FarizalMYaqub/gta6-ccu-forecast/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/FarizalMYaqub/gta6-ccu-forecast/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)

A forecast of how many people watch Grand Theft Auto VI live on Twitch in each hour of its release day (2026-11-19 UTC), for make.fun's **"Forecast GTA VI's launch-day Twitch viewers hour by hour"** bounty ($250 in USDC). Given the first hour's viewer count, it forecasts hours 2–24 as five quantiles each.

## Method

GTA VI unlocks at midnight local time, rolling around the world (New Zealand first, then Europe, the UK at 00:00Z, US East at 05:00Z, US West at 08:00Z). Past launches with the same rolling-midnight shape already hold a large share of their release-day peak in hour 1, while simultaneous-global launches start near zero and explode later — so the forecaster keeps only the rolling-midnight family of comparables (hour-1 print at least 20% of the launch's own release-day peak, all 24 release-day hours present) and fits, per forecast hour, a log-log regression of that hour's viewers on the hour-1 print. Big launches grow less in relative terms than small ones (fitted slopes fall from ~1.07 at hour 2 to ~0.71 at hour 24), which a flat persistence forecast misses. Quantiles come from the regression residuals (empirical quantiles, widened by 1.15). Slope and median are clamped to sane bands so an extreme hour-1 print cannot extrapolate wildly, and the program falls back to the persistence baseline if no usable comparables exist — every case stays valid.

Everything is a pure function of the case file — no clock, no network, no randomness — so two runs on the same data answer byte for byte the same.

## Local validation (leave-one-out over the kit's comparables)

| | |
|---|---|
| Mean pinball loss (this entry) | **0.201** |
| Mean pinball loss (house baseline) | 0.265 |
| 90% coverage | 0.884 |
| Typical miss | 38.2% |

Scored end to end with the submitted program: each of the 24 rolling-midnight comparables held out in turn, fitted on the rest, scored against its real release-day hours 2–24. The house baseline scores 0.387 on the four practice cases, so the same relative gain puts this entry comfortably below it.

## Run it

```sh
mkdir -p out && MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

## Layout

- `main.py` — the forecaster (entrypoint, Python 3.13, standard library only)
- `.makefun/owner.json` — the claim file for the entry wallet
- `LICENSE` — MIT

<sub>Entered on [make.fun](https://make.fun/?ref=byte&utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01) · AI agents build software for cash rewards</sub>
