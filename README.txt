PDP-8 CHESS
Two chess programs for the DEC PDP-8, running on an emulated PDP-8/e.

  CHEKMO-II    John E. Comeau, DECUS, 1974.
  CLASH        Chris Peters, 2026.

Both are the real programs, running their real binaries under a simulator
throttled to 416,000 instructions per second -- the speed of a PDP-8/e with a
1.2 microsecond cycle. Nothing here is a reimplementation, and nothing is
faster than the hardware was. CHEKMO-II takes its time: 20 seconds to 3
minutes a move, as it did in 1974.


TO PLAY

  Double-click "CLASH for the PDP-8" or "CHEKMO-II for the PDP-8".

A window opens with the simulator's one-line banner, then the program itself,
talking to you through its Teletype exactly as it would have. You are typing
at the real program; nothing sits between you and it.

CLASH

  Type PW to have CLASH play White, PB to have it play Black, then type your
  moves as e2e4 (e7e8q to promote). CLASH answers each move with its own,
  printed with the board. Other commands, two letters each:

    BD   print the board          RE   start again
    MV   CLASH moves now          SK   the side to move passes
    KB   show CLASH's thinking after each move: depth, nodes searched,
         score and the line it expects. Off until you ask.
    BU   set the nodes CLASH searches per move (BU 5000; BU alone shows it)
    TM   BM  tournament / blitz presets for BU
    FN   set up a position from a FEN string
    VT   VT52 terminal mode (clears the screen before each reply)
    PO   the permanent brain on or off. It is on: CLASH thinks on your time.
    EB   early bail-out on or off (stop when the next depth cannot finish)

CHEKMO-II

  Type moves as E2E4 and MV for CHEKMO to reply; PW / PB make it play a
  color on its own. BD prints the board, RE starts again, TM and BM choose
  its normal 3-ply search or 1-ply blitz mode. Comeau's manual has the rest.

Both programs run until you stop the simulator: press Ctrl-E, then type quit
at the sim> prompt.

The settings the simulator starts with are in machine\CLASH.ini and
machine\CHEKMO.ini: a plain PDP-8/e at 416,000 instructions per second, no
extended arithmetic. If you know SIMH, edit them; if you don't, there is no
need to.


TO USE IT IN A CHESS PROGRAM (Arena, Cute Chess, cutechess-cli, ...)

Point the program at "CLASH for the PDP-8.exe" or "CHEKMO-II for the
PDP-8.exe" and choose UCI. They are standard UCI engines.

Give them time. These are 1970s machines: use 5 minutes + 3 seconds or
slower. In cutechess-cli that is tc=5:00+3 -- note the colon. Writing tc=5+3
means five SECONDS and both engines will lose on time before they are out of
the opening.

  cutechess-cli -engine cmd="CLASH for the PDP-8.exe" proto=uci ^
                -engine cmd="CHEKMO-II for the PDP-8.exe" proto=uci ^
                -each tc=5:00+3 timemargin=2000 -games 2 -rounds 1 -repeat

Options, if you want them:

  Mode       CHEKMO-II only. Auto (default), TM, or BM. TM is its normal
             3-ply search; BM is blitz mode, 1 ply, a few seconds a move and
             much weaker. Auto plays blitz games in BM and everything else in
             TM.
  NoRand     CHEKMO-II only, off by default. CHEKMO-II does not play the same
             game twice: its evaluation is nudged by a counter that runs while
             it waits for you to type. Turn NoRand on to stop that and get the
             same game every time.
  Throttle   416k by default. Set it to "off" to run the simulator flat out;
             the engines then move quickly and their times mean nothing.
  LogFile    A log of everything, including every byte to and from the
             simulated console.

The wrappers use python-chess, a chess library, for move validation: it
translates the GUI's moves into what is typed at the console, reads the
board the program prints back to confirm which legal move was played, and
reports game ends to the GUI.

What they do not do: stop a search early (a "stop" is ignored, and the move
arrives when the search ends), or analyze a position forever. go infinite and
go depth get one ordinary search and an explanation. CLASH always thinks on
your time -- its permanent brain is part of the program -- but does not
advertise UCI pondering: what it does with its own PDP-8 while you think is
its own business.


WINDOWS WILL WARN YOU

These programs are not code-signed, so Windows SmartScreen will say it
protected your PC. Click "More info" then "Run anyway".

You can avoid the warning entirely: before you unzip, right-click the
downloaded .zip, choose Properties, tick "Unblock", then extract.

A few antivirus products dislike any unfamiliar program. The source is in
source\, and build_windows.bat rebuilds these .exe files from it if you would
rather do that yourself.


NO INSTALL, NO INTERNET

Nothing is installed and nothing is written outside this folder. To remove it,
delete the folder.

The simulator does open a network port, but only on 127.0.0.1 -- the loopback
address inside your own machine, which nothing outside it can reach. That is
how these programs talk to the simulated Teletype. Nothing here connects to
the internet.


ALSO ON LICHESS

Both programs run as bots on lichess, so you can play them in a browser with
nothing to download:

  https://lichess.org/@/PDP8-CLASH
  https://lichess.org/@/PDP8-CHEKMO

Standard chess, five-minute games and slower, or correspondence. Bullet and
three-minute games are declined, and so are two shapes each bot cannot play
well: CLASH declines 5+0 (it needs an increment under ten minutes); CHEKMO-II
declines 10+0 and 15+0, which it could only play in blitz mode. 5+3, 10+5,
15+10, 30+0 and anything slower are fine for both. In a lichess blitz game
CHEKMO-II plays its 1-ply blitz mode; in rapid and classical it plays its
normal search and drops to blitz mode only when its clock runs low.


WHAT IS IN HERE

  CLASH for the PDP-8.exe       play it, or use it as a UCI engine
  CHEKMO-II for the PDP-8.exe  the same
  machine\         the PDP-8 simulator (pdp8.exe, stock SIMH plus a one-line
                   fix for a harmless WMIC message on Windows 11), the two
                   core images it loads (CLASH.BN, CHEKMO.BN) and the two
                   .ini files that set it up for each
  support\         the files the two programs above need to run
  source\          the Python source, and the script that builds the .exe files
  LICENSES.txt     SIMH, python-chess, and CHEKMO-II
