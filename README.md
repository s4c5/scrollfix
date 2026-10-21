# scrollfix

A tiny shell script that resets your terminal to a sane state.

## Symptoms

Your terminal is broken if you see any of these:

- Cursor has disappeared
- Clicking or scrolling produces garbage like `^[[M` or `^[[<35;12;5M`
- Mouse movement floods the terminal with escape sequences
- Pasting text produces garbled output or doubled characters
- Typing feels wrong or input is invisible
- `less`, `vim`, `top`, or another program exited uncleanly and left things messy

Just run `scrollfix` and it goes back to normal.

## Setup

1. Put the `scrollfix` file somewhere in your home directory (e.g. `~/scrollfix`)

2. Make it executable:

```sh
chmod u+x ~/scrollfix
```

3. Run it whenever your terminal is messed up:

```sh
~/scrollfix
```

That's it. No flags, no options.

## What it does

It fixes the terminal **non-destructively** — your screen contents and
scrollback are left untouched. (It deliberately avoids `reset`, which sends a
hard RIS that wipes the screen *and* scrollback.)

1. Bails out if not run on a real terminal (safe to call from scripts/pipes)
2. `stty sane` — restores line discipline (echo, canonical mode, flow control)
3. `DECSTR` soft reset (`\e[!p`) — restores cursor, wrap, origin, insert mode,
   etc. to defaults without clearing anything
4. Ensures you're back on the main screen if a crashed TUI left you on the
   alternate buffer (restores it — does not clear it)
5. Clears the modes a soft reset misses: bracketed paste, focus reporting,
   synchronized output, all mouse tracking, autowrap, keypad, charset
   (line-drawing → ASCII), scroll region, left/right margin mode, colors,
   cursor shape
6. Drains any stale queued input (mouse/focus byte floods) until empty

## Requirements

`bash` (the input-flush loop uses `read -t`/`-n`, which POSIX `sh`/`dash` lack).
The `#!/usr/bin/env bash` shebang handles this on Linux and macOS alike.
