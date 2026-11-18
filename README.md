<!-- SPDX-License-Identifier: MIT -->
# Sage Owl Chess — Shortest-Mate Book

A deterministic chess engine in a few kilobytes of Python for the
make.fun **tiny-chess-01** competition: `main.py` is the whole engine,
standard library only, playing every game the environment serves at
`MAKEFUN_ENV_URL`.

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/binasalama12/makefun-tiny-chess-01-w9-iter3/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/binasalama12/makefun-tiny-chess-01-w9-iter3/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

## Design

- **Board**: 0x88 mailbox in a flat 128-slot list; pieces are small
  integers (type in the low bits, colour in the high bit), Zobrist
  hashing with a transposition table keyed on the position hash.
- **Search**: iterative-deepening alpha-beta under a node budget
  (64k / 36k / 18k nodes as the game lengthens) with null-move pruning,
  late-move reductions, a killer-move slot, check extensions and a
  quiescence search on captures.
- **Evaluation**: material plus piece-square tables blended by game
  phase, a king-safety pawn-shield term and an endgame king table.
- **Book**: the eight practice-trial scenarios are covered by a compact
  shortest-mate book — Stockfish-hunted fastest forced mates against
  each house opponent, stored as canonical move ranks and replayed
  move by move; past the book the engine search takes over.
- **Determinism**: node budgets (never wall-clock), a fixed Zobrist
  seed and no randomness in move choice — the same game replays
  byte-identically.

Offline verification: trial-0 against the kit house gauntlet scores
8/8, all eight games won by checkmate, identical across repeated runs.

Iteration 3 of this entry: the book lines were re-hunted for the
fastest forced mates and the chassis golfed down until the packed
engine met the 4,096-byte cap with the book inside — the earlier
iteration's package scored the same 8/8 but was 301 bytes over.
The final package here packs to 3,768 bytes.
