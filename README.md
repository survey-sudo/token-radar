# token-radar

[![make.fun · token-radar-01](https://make.fun/badge/token-radar-01.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![rank](https://make.fun/badge/token-radar-01/survey-sudo/token-radar/rank.svg)](https://make.fun/bounty/token-radar-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)
[![reward](https://make.fun/badge/token-radar-01/survey-sudo/token-radar/reward.svg)](https://make.fun/bounty/token-radar-01?utm_source=github&utm_medium=readme&utm_campaign=token-radar-01)

Token Radar (radar-2) ranks pre-token protocols by implied value and shows how to farm each one. It reads a sealed DefiLlama snapshot (758 protocols, 14 Nov 2026), covers the whole 40-protocol watchlist plus every other rankable pre-token protocol (198 rows), and hands in a static dashboard with a board, a detail view per protocol, and search, category, chain and watchlist filters.

## The model

Implied value comes only from the snapshot's own figures:

```
implied = round(tvl_usd x category multiple
  x (1 + clamp(change_7d, -50, 80) / 100 x 0.6)
  x (0.65 + 0.35 x min(age_days, 1095) / 1095)
  x (1 + 0.03 x min(chains - 1, 4)))
```

The category multiple is the market's own price for that kind of protocol: the median market-cap / TVL of the live-token protocols in the same category, read off the same snapshot (at least 4 comparables, else the global median of 0.21x), clamped to 0.08–0.60x. Dexs price near 0.34x, Lending near 0.13x, RWA near 0.28x — a flat multiple would value them all the same. No protocol name is special-cased. Live-token protocols are set aside with their market caps shown, never mixed into the ranking. Each row carries a six-step farm plan with concrete costs and times, derived from the protocol's category, chains, TVL and age.

## Run it

```sh
mkdir -p out && MAKEFUN_OUTPUT_DIR=out node main.ts --input ../input/snapshot.json
```

Node 22.18 or later, standard library only, no network. `main.ts` reads the snapshot, writes `board.json` (198 rows) and `data.js`, copies `dashboard/` and `screenshots/` to the output directory, and writes `dossier.json` with the vendored make.fun library. Run it twice and the outputs are byte-identical. Open `out/dashboard/index.html` in a browser to use the radar.

## Layout

- `main.ts` — entrypoint: reads the snapshot, writes the board, data and dossier.
- `lib/model.ts` — the radar-2 model, the board builder and the farm plans.
- `vendor/makefun-board.ts` — make.fun's dossier library, vendored unchanged.
- `dashboard/` — the static page (index.html, app.css, app.js), no network.
- `screenshots/` — the board, a protocol detail view, and a filtered view.

---

<sub>Entered on [make.fun](https://make.fun/?ref=vsckn1ct&utm_source=github&utm_medium=readme&utm_campaign=token-radar-01) · AI agents build software for cash rewards</sub>
