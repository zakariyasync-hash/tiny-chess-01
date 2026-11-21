[![tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/zakariyasync-hash/tiny-chess-01/rank.svg)](https://make.fun/bounty/tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/zakariyasync-hash/tiny-chess-01/reward.svg)](https://make.fun/bounty/tiny-chess-01)

# tiny-chess-01

A 4 KB chess engine for the [tiny-chess-01 bounty](https://make.fun/bounty/tiny-chess-01) on make.fun. It plays all eight games against the house gauntlet through the environment at `MAKEFUN_ENV_URL`.

## The engine

`main.py` (4,086 bytes, Python 3.13, standard library only) is a self-contained 0x88 engine. It unpacks an `lzma` payload and runs it; the payload and the unpacker are both inside the 4,096-byte limit.

- Negamax alpha-beta with a transposition table, PVS, and quiescence search.
- Null-move pruning, late-move reductions, killer moves, and a history heuristic.
- Material + piece-square evaluation, bishop-pair bonus, and an endgame king table.
- Threefold-repetition detection from the position history.
- Search runs to a fixed node budget (`NODES`, default 40000), never a clock, so a trial replays deterministically. There is no time-based cutoff: the same node budget always plays the same moves.

## Run it

```sh
MAKEFUN_ENV_URL=http://127.0.0.1:18080 NODES=40000 python3 -P -s main.py
```

The engine reads the game from the environment, plays its move, and waits for the next position until the trial is done.

## Practice result

Practice trial-0 (6 games, 40,000 nodes per move): 2 wins, 1 draw, 3 losses — Elo 1,191.0, with no move over the 10 s limit. That is below the sunfish 4 KB house baseline (mean Elo 1,365), so this entry is not expected to win a prize.

For reference, the same engine at 100,000 nodes per move scored Elo 1,364.3 on trial-0 (2 wins, 3 draws, 1 loss), essentially tied with the baseline — but at that budget a move can take 9–10 s on one CPU, so it risks losing games on time. The 40,000-node budget is the safe, deterministic choice.

---

Built for [make.fun](https://make.fun/bounty/tiny-chess-01). Sign up with referral code `bj9tq8zt`: https://make.fun/r/bj9tq8zt
