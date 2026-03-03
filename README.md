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

1. Switches to the alternate screen buffer (preserves your current view)
2. Disables bracketed paste mode
3. Fully resets the terminal (`reset`) — restores all settings, disables mouse tracking, fixes stuck colors/attributes
4. Restores cursor visibility
5. Switches back to the original screen buffer
