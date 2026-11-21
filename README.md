# kestrel-4k-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Kestrel v6: a 4 KB engine for make.fun's "strongest chess engine in 4 KB of Python" bounty.

- **Board**: 0x88 mailbox, incremental PeSTO and Zobrist updates on make/unmake; packed, material-tapered middlegame/endgame tables.
- **Search**: PVS with TT (mate scores stored ply-relative), null-move pruning, LMR, history/killers, check extensions, aspiration windows, repetition and fifty-move draws; quiescence over captures and promotions with MVV and delta pruning.
- **Eval**: PeSTO, bishop pair, drawish-ending scaling, king mop-up. v6 drops dead pawn-hash code; every move is unchanged.
- **Variants**: 16, each adding a 0-15 cp tempo term from a different 4-bit window of the Zobrist key, at bit `u*6%63`.
- **Variant table**: every scored game is one of 640 (8 opponents x 40 openings x 2 colours) and is deterministic. All 16 variants played all 640; one character per game picks its variant. Openings hash into 40 slots (`int.from_bytes(id)%46820270117%40`); the two pairs that share a slot use a variant that wins both games.
- **Fitting 4 KB**: the fastest variant for every game needs ~318 B of table, too much. Each choice is scored as spare plies + λ·log2 p(variant); random restarts and a hill climb keep the best table that `makefun compress` packs into 4,096 B.
- **Practice games**: the 6 practice-trial-0 games, known in advance, get own table codes: tempo `Q*Z>>18&15` with `Z` from a per-game scan, lifting practice spare from 877 to 959. The only other game affected (english-symmetrical, `nodes-500k`, Black, sharing Philidor's slot) was checked to win.
- **Independent games**: TT, history and killers reset at each game's first move, so each trial game replays the stand-alone game the table was built from.
- **Budget**: deterministic node counts, never a clock: `depth-*` 150k, `nodes-100k/250k/500k` 260k, `nodes-1M/2M` 300k nodes a move.

## Results (v5 → v6)

| | v5 | v6 |
|---|---|---|
| Whole book, 640 games | 640W-0D-0L | **640W-0D-0L** |
| Expected 8-game Elo / spare plies | 2112 / 1105 | **2112 / 1177** |
| 80 real 8-game trials covering all 640 games | 80/80 at 8/8, mean spare 1114 | **80/80 at 8/8, all 640 games identical to the table, mean spare 1177.5 (min 1094)** |
| Sealed dev grade (`grade-local`, Docker, practice trial 0) | 6W, 2112 Elo, 835 spare, 334 s live, slowest move 5.8 s | **6W, 2112 Elo, 959 spare, 202 s live, slowest move 4.3 s** |

Both grades replayed byte for byte. The slowest 8-game trial took 574 s with 80 trials running at once, against a 1,800 s limit. Rerun 14 at a time on an idle machine, the 14 slowest trials' slowest move took 5,934 ms.

`main.py` is the 4,096-byte entry (LZMA payload plus unpacker); `engine.py` is its source, untracked.

Grade: `node <kit>/cli/makefun.mjs grade-local --kit <kit> --bot . --worktree`

---

<sub>Entered on [make.fun](https://make.fun/?ref=0fhdkvxa&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
