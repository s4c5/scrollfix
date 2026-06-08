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

1. Bails out if not run on a real terminal (safe to call from scripts/pipes)
2. Switches to the alternate screen buffer (preserves your current view)
3. Runs the big hammer: `stty sane` + `reset`
4. Clears DEC private modes `reset` misses, *after* reset so they can't get
   re-dirtied: bracketed paste, focus reporting, synchronized output, all mouse
   tracking modes, autowrap, charset (line-drawing → ASCII), colors, cursor shape
5. Drains any stale queued input (mouse/focus byte floods) until empty
6. Switches back to the original screen and resets its scroll region + cursor

## Requirements

`bash` (the input-flush loop uses `read -t`/`-n`, which POSIX `sh`/`dash` lack).
The `#!/usr/bin/env bash` shebang handles this on Linux and macOS alike.
