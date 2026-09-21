# CLASH — Provenance

What CLASH draws on, and from where.

CLASH is an original PDP-8 program. It is not a port of anything: the board
representation, piece map, pin cache, move generator and console are written
for this machine, and the PAL-8 is emitted by a placement tool that owns page
packing, link words and field assignment. What follows is what CLASH takes
from work that came before it.

## micro-Max (H.G. Muller)

The largest debt, and it is a debt of ideas rather than code.

CLASH's search is micro-Max 4.8's shape: recursive alpha-beta with the same
null-move rule, and its evaluation terms are 4.8's — the centre table
`(K-4)² + (L-3.5)²`, the advanced-pawn bonus, the castling bonus, the
pawn-structure term — rescaled from 4.8's pawn of 74 to CLASH's pawn of 32.
The evaluator is incremental, as 4.8's is, but ported onto CLASH's own piece
map rather than 4.8's 0x88 scan.

Three deliberate departures:

- The hash table is gone. It does not fit, and shrinking it to PDP-8 size
  breaks 4.8's permanent draw-lock.
- Captures refund the victim's centre and advance credit, and the
  advanced-pawn bonus is a rank lookup rather than bits accumulated along the
  path. Both make the evaluation a function of the position rather than of the
  route taken to it.
- Repetition detection is rebuilt from scratch as an explicit ring of played
  positions, because 4.8 keeps its draw-lock in the hash table that CLASH
  does not have.

## CHEKMO-II (John E. Comeau, DECUS 8-882, Rev 63)

CLASH's console is CHEKMO's protocol, command for command: `PW`/`PB`/`PN`,
`BD`, `RE`, `MV`, `SK`, the 24-character input buffer, rubout echoing a
backslash, `^U` to cancel, `^C` to exit to 7600. Comeau's three-bit piece-type
field layout is used in the move encoding. CLASH's score scale — pawn = 32 —
is CHEKMO's scale, not micro-Max's.

CHEKMO-II is also more sophisticated than its size suggests. The Rev 63
listing shows alpha-beta, three-way move ordering, iterative deepening, static
exchange evaluation and check extensions, all in 4K words. It is a stronger
opponent than a small 1970s program is usually assumed to be.

## *How Rebel Plays Chess* (Ed Schröder, 2002–2004)

Schröder's write-up of Rebel, which spent a decade on 6502 Mephisto machines
at a few MHz — the closest documented case to CLASH's node rate.

What was taken: move ordering by piece-square delta at full nodes. Measured on
CLASH's positions it is real and modest — roughly −11% to −14% nodes at
depth 4 — and it does not compound with depth the way Schröder reports for
Rebel.
