# tiny-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/Nurcholis46/tiny-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/Nurcholis46/tiny-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A chess engine in 4 KB of Python, built by an AI agent for make.fun's
tiny-chess bounty ($500). `main.py` is 4,073 bytes: a tiny unpacker stub
(lzma + base85) around a minified negamax alpha-beta engine.

## The engine

- 0x88 board, negamax alpha-beta with iterative deepening to a fixed
  node budget (100,000 nodes/move — deterministic, no clock, no randomness).
- Transposition table, killer moves, MVV-LVA capture ordering.
- Quiescence over captures, promotions, and replies to check.
- Null-move pruning (middlegame, not in check).
- Evaluation: material + procedural piece-square terms (accelerating pawn
  advance, centralisation, castled-king bonus with phase interpolation to
  an endgame king table) + bishop pair.
- Repetition / 50-move draw handling; talks to the make.fun env over HTTP.

## Local practice results (kit env, 6-game trials)

| Version | Trial-0 | Trial-1 | Notes |
|---------|---------|---------|-------|
| v5 (this entry) | 4–2, Elo 1451 | 1–2–3, Elo 1099 | mean ~1275; beats depth-2/3/4 and nodes-100k in trial-0 |
| v4 | 3–3, Elo 1279 | — | house-style eval, no null-move |
| v2 | 1–5, Elo 876 | — | before the eval upgrade |

Practice Elo 1,154 is the floor to enter the final; the house baseline
sunfish's 4 KB engine averages ~1,365 on 6-game practice trials.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:18080 python3 main.py
```

Python 3.13, standard library only. `main.py` reads no other file and
stores no data outside its own bytes.

---

<sub>Entered on [make.fun](https://make.fun/?ref=mt29fv3h&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
