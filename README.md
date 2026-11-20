# kestrel-4k-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Kestrel v3: a 4 KB engine for make.fun's "strongest chess engine in 4 KB of Python" bounty.

- **Board**: 0x88 mailbox, incremental PeSTO and Zobrist updates on make/unmake; packed, material-tapered middlegame/endgame tables.
- **Pawn hash**: pawn structure, king shelter, doubled/isolated and passed pawns cached, so pawn-only moves skip work.
- **Search**: PVS with TT, null-move pruning, log-table LMR, history/killers, check extensions, aspiration windows, repetition and fifty-move draws.
- **Quiescence**: captures and promotions with MVV pruning and delta margins; king mop-up endgames.
- **Budget**: deterministic node counts, never a clock. `depth-*` 150k, `nodes-*` 240k and `nodes-1M/2M` 340k nodes a move; the 8-game scored trials scale the three depth rungs to 75% (112k) to leave wall time for the two extra heavy rungs, all under a 90M-node governor.
- **Deterministic**: seeded Zobrist tables, node counts only; trials replay byte for byte.
- **Sealed dev grade**: `6W-0D-0L`, Elo 2112, 795 spare plies, 403 s live, 0 timeouts, replay matched.
- **Random scored gauntlet** (48 eight-game instances, 384 games): 368W-13D-3L (95.8% wins, 99.2% unbeaten); 100% vs `depth-2/3/4`, 97.9% vs `nodes-100k`, 95.8% vs `nodes-250k`, 89.6% vs `nodes-500k`, 91.7% vs `nodes-1M` and `nodes-2M`; every win by checkmate, peak RSS 156 MB.

`main.py` is the 4,089-byte entry (LZMA payload plus unpacker); `engine.py` is its source, untracked.

Grade: `node tiny-chess-01-kit/cli/makefun.mjs grade-local --source . --worktree`

---

<sub>Entered on [make.fun](https://make.fun/?ref=0fhdkvxa&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
