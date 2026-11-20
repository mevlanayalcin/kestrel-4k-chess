# kestrel-4k-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Kestrel v4: a 4 KB engine for make.fun's "strongest chess engine in 4 KB of Python" bounty.

- **Board**: 0x88 mailbox, incremental PeSTO and Zobrist updates on make/unmake; packed, material-tapered middlegame/endgame tables.
- **Pawn hash**: pawn structure, king shelter, doubled/isolated and passed pawns cached, so pawn-only moves skip work.
- **Search**: PVS with TT (mate scores stored ply-relative), null-move pruning, log-table LMR, history/killers, check extensions, aspiration windows, repetition and fifty-move draws.
- **Quiescence**: captures and promotions with MVV pruning and delta margins.
- **Mop-up**: drives the losing king to the edge and brings our king in once ahead by 200 cp against a pawnless side, or by 500 cp while the defender still has pawns (v3 only did the pawnless case).
- **Budget**: deterministic node counts, never a clock. `depth-*` 150k, `nodes-100k/250k/500k` 280k (v3: 240k) and `nodes-1M/2M` 340k nodes a move; the 8-game scored trials scale the three depth rungs to 75% (112k), all under a 90M-node trial governor.
- **Deterministic**: seeded Zobrist tables, node counts only; trials replay byte for byte.
- **Sealed dev grade** (`grade-local`, Docker): `6W-0D-0L`, Elo 2112, 801 spare plies (v3: 795), 528 s live, 0 timeouts, replay matched; slowest move 7.1 s.
- **Whole-book evaluation** (all 640 scored-trial games: 8 opponents x 40 openings x 2 colours, each from a fresh process): 624W-13D-3L, expected 8-game scored Elo 2082 and spare plies 889 (v3 on the same 640 games: 612W-16D-12L, 2048, 862). Against `nodes-1M` 78/1/1 and `nodes-2M` 72/8/0 (v3: 73/3/4 and 72/6/2).
- **8-game trials at the real 10 s move limit** (10 scored instances): 80W-0D-0L, mean Elo 2112, mean spare 909 (v3: 75W-3D-2L, 2025, 867); slowest trial 1,110 s, peak RSS 164 MB.

`main.py` is the 4,075-byte entry (LZMA payload plus unpacker); `engine.py` is its source, untracked.

Grade: `node tiny-chess-01-kit/cli/makefun.mjs grade-local --source . --worktree`

---

<sub>Entered on [make.fun](https://make.fun/?ref=0fhdkvxa&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
