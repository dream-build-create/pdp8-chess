# PDP-8 Chess

Two chess programs for the DEC PDP-8, running on an emulated PDP-8/e.

| | | Memory |
|---|---|---|
| **CHEKMO-II** | John E. Comeau, DECUS, 1974 | 4K words |
| **CLASH** | Chris Peters, 2026. Version 1.00 | 16K words |

Both are the real programs: their actual binaries, running under a PDP-8
simulator throttled to 416,000 instructions a second, the speed of a PDP-8/e
with a 1.2 µs cycle. Nothing is reimplemented and nothing is faster than the
hardware was.

## Download

**[Releases](../../releases)** → `PDP-8 Chess.zip`. Unzip it anywhere.
Windows, no install, nothing written outside the folder, no internet.

## Two ways to run them

**Double-click** `CLASH for the PDP-8.exe` or `CHEKMO-II for the PDP-8.exe`.
A window opens and you are at the program's own Teletype, exactly as in 1974
or on the machine CLASH was written for. Type `PW` and CLASH plays White
against you; type `E2E4` and `MV` and CHEKMO-II answers. `README.txt` lists
the commands.

**As a UCI engine** in Arena, Cute Chess, cutechess-cli or anything else that
speaks UCI: point it at either `.exe`. Use 5 minutes + 3 seconds or slower;
these are 1970s machines.

You can also play them right now on lichess.org with nothing to download:
choose "Play with a friend" and enter
[PDP8-CLASH](https://lichess.org/@/PDP8-CLASH) or
[PDP8-CHEKMO](https://lichess.org/@/PDP8-CHEKMO) as the friend. They accept
five-minute games and slower, with a few exceptions listed in `README.txt`.

## What is in the repository

```
README.txt        the notes that ship in the zip: commands, options, caveats
source/           clash_uci.py, chekmo_uci.py, and the script that builds
                  the two .exe files from them
machine/          the PDP-8 simulator (SIMH), the two core images
                  CLASH.BN and CHEKMO.BN, and the .ini files that set the
                  simulator up for each
LICENSES.txt      SIMH, python-chess, CHEKMO-II
```

The `.exe` files are only in the release zip: they are the Python sources
above packaged with PyInstaller, and `source/build_windows.bat` rebuilds
them from scratch if you would rather not run a downloaded binary.

The wrappers use python-chess, a chess library, for move validation: it
translates the GUI's moves into what is typed at the console, reads the
board the program prints back to confirm which legal move was played, and
reports checkmate, stalemate and other game ends to the GUI.

## What is in CLASH

Standard techniques, most of them decades newer than the machine, fitted
into a PDP-8 with no hardware stack, no multiply, and a 12-bit word:

- **Iterative deepening** with alpha-beta search, principal variation search,
  and killer-move and piece-square move ordering. At PDP-8/e speed a typical
  move reaches three or four ply; the search goes deeper when the position
  narrows.
- **Quiescence search** of captures and promotions at the leaves, with a
  check extension, so it does not stop thinking in the middle of an exchange.
- **Null-move pruning.**
- **Pondering** -- thinking on the opponent's time.
- **A rudimentary opening book**, three moves deep.
- **Repetition awareness** -- it knows when a position has occurred before,
  avoids repeating when it is ahead, and repeats when it is behind.
- **An evaluation** of material, piece placement, mobility and pawn
  structure, including passed pawns on the seventh rank and blocked pawns.
- **Time control by node count.** CLASH has no clock and reads none. It is
  told how many positions to search for a move (BU on the console; the UCI
  wrapper converts the clock into that number), which keeps the program
  identical on a simulator, a PiDP-8, or a real PDP-8.
- **A console** in CHEKMO-II's two-letter style: moves in coordinate
  notation, a board that reads on an ASR-33, FEN position entry, an optional
  line after each move showing depth, nodes, score and the expected line,
  and a VT52 mode.

It is written in PDP-8 assembly language, 16K words, and runs standalone from
the BIN loader with nothing under it.

## How well do they play

Each program played 20 games against the lichess computer opponent at
level 5 (Stockfish, as lichess limits it for that level), rapid 15+10, ten as
White and ten as Black, in September 2026, at PDP-8/e speed.

| | Wins | Losses | Draws | Score |
|---|---|---|---|---|
| **CLASH** | 11 | 6 | 3 | 12.5 / 20 |
| **CHEKMO-II** | 0 | 18 | 2 | 1 / 20 |

The bots are on lichess and the programs are in the zip; try them against
whatever you like.

## Background

CHEKMO-II is one of the earliest chess programs written for a minicomputer,
and a remarkable one: a complete game of chess, with castling, en passant,
promotion, and a three-ply search, in 4,096 twelve-bit words of memory, on a
machine that ran under a million instructions a second and had no multiply
instruction. John Comeau wrote it in 1974 and gave it away through the DECUS
program library. It has been the chess program for the PDP-8 ever since,
and for fifty years nothing else seriously tried.

CLASH was written in 2026 for the same machine, at the same speed, as an
experiment: would fifty years of chess programming knowledge, learned on
computers thousands of times faster, translate back to this iconic machine?
