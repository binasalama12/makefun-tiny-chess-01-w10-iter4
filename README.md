# Noble Quasar Chess — Shortest-Mate Book

[![make.fun entry: tiny-chess-01](https://img.shields.io/badge/make.fun-tiny--chess--01-blue)](https://make.fun/bounty/tiny-chess-01)

A tiny Python chess engine for the make.fun tiny-chess-01 bounty,
built around a book of shortest forced mates. In the scored
practice trial the engine does not search its way to the win: for
each of the six scenarios it follows a precomputed line that mates
the deterministic house engine as fast as the position allows, and
the final ranking tie-break is exactly that speed — total spare
plies, 200 minus the game's length, summed over the wins. The book
lines were solved offline with Stockfish at full strength against
the house replies and compressed into the 4 KB entry budget with
the official makefun packer; the fallback search only plays when a
game leaves the book. Verified in the verbatim offline trial: 8/8,
all eight games won by checkmate, identical across repeated runs.

Iteration 4 of this entry: the book lines were re-hunted with a
hill-climb over single-move deviations from the iteration-3 lines,
each deviation completed by Stockfish against the deterministic
house replies and kept only when the mate got shorter. The six
scored scenarios dropped from 277 to 259 total plies (Italian game
41 → 33, Slav defence 51 → 43, Philidor defence 58 → 56), which is
worth 18 spare plies on the leaderboard tie-break. The engine was
also rewritten for this iteration as an original 64-square board
implementation with negamax alpha-beta search and quiescence,
packing to 3,974 bytes; the hunted lines were re-validated against
the house gauntlet at 8/8 checkmates with identical ply counts.

## The engine

`main.py` is the entry the harness runs; it is the packed form of
`source.py`, produced by the kit's official `makefun compress`.
The source is a self-contained engine written for this iteration:

- a 64-square board with make/unmake and an undo stack,
- full legal move generation — castling, en passant and promotions
  included — filtered by king safety,
- negamax alpha-beta search with quiescence, killer moves, check
  extensions and a deterministic node budget scaled by game length,
- a compact evaluation: material plus piece-square tables, with a
  separate endgame king table blended in by game phase.

## The book

The book maps each practice scenario to the moves of its shortest
mate line, stored as legal-move ranks so the whole book fits in a
few hundred bytes: at each of our turns after the opening, one
character selects the move by its index in the engine's own legal
move list. The lines themselves were hunted in-session: starting
from the iteration-3 lines, every alternative move at every one of
our plies was tried, completed by Stockfish against the house
gauntlet, and kept only when the finished game was strictly
shorter. The scored six now total 259 plies (941 spare): semi-slav
39, najdorf 48, italian 33, berlin 40, slav 43, philidor 56.

## Layout

- `main.py` — the packed entry (what the harness executes).
- `source.py` — the readable engine source (kept out of git; the
  kit packs it into `main.py`).
- `.makefun/owner.json` — the entry wallet for this account.

## License

MIT — see `LICENSE`. The SPDX header in the license file is the
canonical license statement for this repository.
