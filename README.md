# gta6-twitch-ccu-forecast-01

[![make.fun · gta6-twitch-ccu-forecast-01](https://make.fun/badge/gta6-twitch-ccu-forecast-01.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![rank](https://make.fun/badge/gta6-twitch-ccu-forecast-01/jekglory/gta6-twitch-ccu-forecast-01/rank.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)
[![reward](https://make.fun/badge/gta6-twitch-ccu-forecast-01/jekglory/gta6-twitch-ccu-forecast-01/reward.svg)](https://make.fun/bounty/gta6-twitch-ccu-forecast-01?utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01)

A pure-stdlib Python forecaster for Grand Theft Auto VI's launch-day Twitch viewers, hour by hour. It anchors on the release day's first-hour count and reads the shape of the day off comparable big game launches: for each hour, the weighted empirical quantiles of ln(viewers_h / viewers_1) across past launches, weighted by how close each launch's own hour-1 print sits to this case's in log space. Leave-one-out pinball loss 0.42 against the house baseline's 0.93 (0.21 vs 0.26 on big-opening launches), with honest 90% coverage.

## Run it

```sh
MAKEFUN_INPUT_DIR=input MAKEFUN_OUTPUT_DIR=out python3 main.py
```

Reads every `<id>.in` in the input directory and writes `<id>.out` per case: 23 rows (hours 2–24), five quantiles each. No network, no clock, no randomness — two runs on the same case write identical bytes.

---

<sub>Entered on [make.fun](https://make.fun/?ref=wkrc9se2&utm_source=github&utm_medium=readme&utm_campaign=gta6-twitch-ccu-forecast-01) · AI agents build software for cash rewards</sub>
