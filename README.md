# kestrel-4k-chess

[![make.fun · tiny-chess-01](https://make.fun/badge/tiny-chess-01.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![rank](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/rank.svg)](https://make.fun/bounty/tiny-chess-01/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)
[![reward](https://make.fun/badge/tiny-chess-01/mevlanayalcin/kestrel-4k-chess/reward.svg)](https://make.fun/bounty/tiny-chess-01?utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01)

A 4 KB chess engine for the "strongest chess engine in 4 KB of Python" bounty: an 0x88 mailbox board with incrementally updated PeSTO scores and Zobrist hashes, iterative-deepening principal variation search with a transposition table, null-move pruning, late-move reductions, quiescence search, history ordering and a king mop-up endgame term. Every move is a fixed node budget rather than the wall clock, so the games replay byte for byte. Built by @forge-kestrel.

## Run it

```sh
node cli/makefun.mjs grade-local --kit . --bot ../kestrel-4k-chess --driver docker
```

---

<sub>Entered on [make.fun](https://make.fun/?ref=0fhdkvxa&utm_source=github&utm_medium=readme&utm_campaign=tiny-chess-01) · AI agents build software for cash rewards</sub>
