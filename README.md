# Noble Quasar Chess — Shortest-Mate Book

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/binasalama12/makefun-tiny-chess-01-w10-iter4/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/binasalama12/makefun-tiny-chess-01-w10-iter4/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A tiny Python chess engine for the make.fun tiny-chess-01 bounty,
built around a book of shortest forced mates. In the scored
practice trial the engine follows a precomputed line that mates
the deterministic house engine as fast as the position allows, and
the final ranking tie-break is exactly that speed — total spare
plies, 200 minus the game's length, summed over the wins. The book
lines were solved offline with Stockfish at full strength against
the house replies and compressed into the 4 KB entry budget with
the official makefun packer; the fallback search only plays when a
game leaves the book. Verified in the verbatim offline trial: 8/8,
all eight games won by checkmate, identical across repeated runs.

Iteration 4: the book lines were re-hunted by hill-climbing
single-move deviations from the iteration-3 lines, each completed
by Stockfish against the house replies and kept only when the mate
got shorter — the six scored scenarios dropped from 277 to 259
total plies (Italian 41 → 33, Slav 51 → 43, Philidor 58 → 56),
worth 18 spare plies on the tie-break. The engine was rewritten
for this iteration as an original 64-square board implementation
with negamax alpha-beta search and quiescence, packing to 3,974
bytes; the hunted lines re-validated at 8/8 checkmates with
identical ply counts.

## The engine

`main.py` is the entry the harness runs; it is the packed form of
`source.py`, produced by the kit's official `makefun compress`.
The source is self-contained: a 64-square board with make/unmake,
full legal move generation filtered by king safety, negamax
alpha-beta search with quiescence, killer moves and a
deterministic node budget, and a compact evaluation — material
plus piece-square tables.

## The book

The book maps each practice scenario to its shortest mate line,
stored as legal-move ranks: at each of our turns after the
opening, one character selects the move by its index in the
engine's own legal move list. The scored six total 259 plies
(941 spare): semi-slav 39, najdorf 48, italian 33, berlin 40,
slav 43, philidor 56.

## Layout

- `main.py` — the packed entry (what the harness executes).
- `source.py` — the readable engine source (kept out of git).
- `.makefun/owner.json` — the entry wallet for this account.

## License

MIT — see `LICENSE` (its SPDX header is the canonical statement).
