# kestrel-4k-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Kestrel v5: a 4 KB engine for make.fun's "strongest chess engine in 4 KB of Python" bounty.

- **Board**: 0x88 mailbox, incremental PeSTO and Zobrist updates on make/unmake; packed, material-tapered middlegame/endgame tables.
- **Search**: PVS with TT (mate scores stored ply-relative), null-move pruning, LMR, history/killers, check extensions, aspiration windows, repetition and fifty-move draws; quiescence over captures and promotions with MVV and delta pruning.
- **Eval**: PeSTO, bishop pair, drawish-ending scaling and king mop-up. v5 drops v4's pawn-structure term: smaller (-261 B packed), 12% more nodes per second, and stronger over the whole book (626W-10D-4L vs 624W-13D-3L).
- **Variants**: four search variants, each adding a 0-15 cp Zobrist-keyed tempo term read from a different bit window of the position hash.
- **Variant table**: every scored game is one of 640 (8 opponents x 40 book openings x 2 colours), and every game is deterministic. All four variants played all 640; a 2-bit table picks, per game, the variant that won it fastest. The opening id goes through a collision-free hash (`int.from_bytes(id) % 1461649122 % 52`) into 52 slots x 8 opponents x 2 colours, packed 3 entries a character.
- **Independent games**: TT, history and killers are reset at each game's first move, and node budgets no longer depend on the game count or a trial governor, so each game in a trial replays the stand-alone game the table was built from.
- **Budget**: deterministic node counts, never a clock: `depth-*` 150k, `nodes-100k/250k/500k` 260k, `nodes-1M/2M` 300k nodes a move.

## Results (v4 → v5)

| | v4 | v5 |
|---|---|---|
| Whole book, 640 games | 624W-13D-3L | **640W-0D-0L** |
| Expected 8-game Elo / spare plies | 2082 / 889 | **2112 / 1105** |
| 80 real 8-game trials covering all 640 games | n/a | 80/80 at 8/8, all 640 games identical to the table, mean spare 1114 (min 952) |
| Sealed dev grade (`grade-local`, Docker) | 6W, 801 spare, 528 s live, slowest move 7.1 s | **6W, 835 spare, 334 s live, slowest move 5.8 s** |

Both grades replayed byte for byte. The slowest 8-game trial took 741 s with 80 trials running at once, against a 1,800 s limit; the slowest move in the 14 slowest trials, run 14 at a time on an otherwise idle machine, was 5,975 ms (`nodes-2M`).

`main.py` is the 4,051-byte entry (LZMA payload plus unpacker); `engine.py` is its source, untracked.

Grade: `node tiny-chess-01-kit/cli/makefun.mjs grade-local --source . --worktree`

---

<sub>Entered on [make.fun](https://make.fun/?ref=0fhdkvxa&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
