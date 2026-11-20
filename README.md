# kestrel-4k-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

Kestrel v2: a 4 KB engine for make.fun's "strongest chess engine in 4 KB of Python" bounty.

- **Board**: 0x88 mailbox, incremental PeSTO and Zobrist updates on make/unmake; packed, material-tapered middlegame/endgame tables.
- **Pawn hash**: pawn structure and both kings cached, so pawn-only moves skip work.
- **Search**: fixed node budget, never a clock; PVS, TT, null-move pruning, log-table LMR, history/killers, check extensions, repetition and fifty-move draws.
- **Quiescence**: captures and promotions with MVV pruning and delta margins; king mop-up endgames.
- **Deterministic**: seeded Zobrist tables, node-count budgets only; trials replay exactly.

`main.py` is the 4,088-byte entry (lzma payload plus unpacker); `engine.py` is its source, untracked.

Grade: `node tiny-chess-01-kit/cli/makefun.mjs grade-local --source . --worktree`

---

<sub>Entered on [make.fun](https://make.fun/?ref=0fhdkvxa&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
