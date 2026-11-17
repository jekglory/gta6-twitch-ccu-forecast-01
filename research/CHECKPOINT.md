# gta6-twitch-ccu-forecast-01 — checkpoint

## Competition
- Slug `gta6-twitch-ccu-forecast-01`, $250 USDC, entries close 2026-11-19T01:00:00Z (08:00 WIB 19 Nov), result due 2026-12-04.
- Program: Python 3.13 `main.py`, stdlib only, reads `/input/<id>.in`, writes `/output/<id>.out` — 23 rows (hours 2..24), five quantiles each. Pinball loss on log scale. Max 8s / 1 CPU / 1024 MB per case, deterministic.
- House baseline (flat at hour-1 print, spread 0.15*sqrt(h-1)): practice loss ~0.387. Prize needs a loss strictly below the best house entry (baseline or house AI model) on the final.

## Data (kit input/p00.in, 38 comparables + GTA VI own pre-release history)
- Every comparable has a complete 24h release day. Hour-1 prints span 164 to 875,155 viewers: unlock timing drives the shape, so the hour-1 anchor's *size* is the conditioning variable.
- Big launches (v1 >= 100k, n=16) median log-ratio shape: dip overnight to ~0.51x at h11, rise to ~1.28x evening peak h21 — the expected diurnal shape for GTA VI (rolling midnight unlock, US prime time in the UTC evening).

## Method (shipped in main.py)
- For each hour h, collect log-ratios r = ln(v_h / v_1) across comparable launches; weight each launch by a Gaussian kernel on ln(v_1) similarity to the case's hour-1 print (bandwidth 1.0, no floor); take weighted empirical quantiles at 0.05/0.25/0.5/0.75/0.95; forecast q = p1 * exp(quantile).
- Adaptive guard: widen bandwidth (1.0 -> 1.5 -> 2.0 -> 3.0) only if effective sample size < 5 (protects a final hour-1 print beyond every comparable).
- Fallback: house-baseline formula for any hour with no usable comparable; robust p1 fallback if the known row is missing.
- Comparables are kept only if hour 1 is present and >= 20 of 24 release-day hours are present (this excludes the GTA VI own-history record, which is pre-release, not a launch).

## Leave-one-out validation (37 comparable launches as pseudo-targets, shipped code)
- House baseline: loss 0.9296 overall / 0.2611 on big-v1 targets / 0.3488 on practice-like v1 (20k-300k).
- Chosen model (bw=1.0, floor 0, adaptive guard min eff-n 5): loss **0.4198** overall / **0.2144** big-v1 / **0.2943** practice-like; 90% coverage 0.864 overall, 0.902 big-v1 (honest spread).
- Prototype without the guard scored 0.4100 overall; the guard costs 0.01 on tiny-v1 targets only (sparse neighborhoods) and is neutral on big/mid targets. It fires only for hour-1 prints above ~2.7M (effective n at bw=1.0: 21.4 at 200k, 15.7 at 500k, 9.6 at 1M, 7.2 at 1.5M, 6.0 at 2M, 4.8 at 3M).
- Extreme-print test: synthetic p1=2,000,000 case runs, valid output (h2 median 1,829,237; h21 median 1,871,124; h21 q05/q95 1,729,023/2,948,529).
- Practice cases p00-p03 run in 0.2s total, two runs byte-identical, all outputs valid shape (23 rows, 5 non-decreasing quantiles, 0 < q <= 1e9).
- Rejected: uniform floor on weights (0.49-0.55 overall — pulls in tiny-v1 launches with huge ramps), normal-approx spreads (0.51-0.65), console boost (no gain), spread shrinkage alpha<1 (no gain), dropping tiny-v1 comps from the prior (0.448).
- Worst LOO targets are tiny-v1 unlock-timing outliers (Wukong, Marathon, Silksong) — not GTA-like; best are the biggest launches (Elden Ring 0.0985, TLOU2 0.1277, Hogwarts 0.1325).

## Repo / submit plan
- Repo: jekglory/gta6-twitch-ccu-forecast-01 (public, MIT, created after 2026-11-17T06:00:00Z open), files main.py, README.md (badges), LICENSE (SPDX MIT), .makefun/owner.json (wallet 8rCWeWgLhbrjRJnW3sLDfPBVLR481kFnoGVsghPPCJjG).
- Bundle with the GTA kit CLI + --muse-usage, run kit check, submit on 0 FAIL. Quota today (2026-11-17): 1 submission used (btc-race-0603), 9 left of 10/day.
- Claim code + submission ID go ONLY in ~/workspace/makefun/submissions-2026-11-13.md. Never claim a prize.
