# auto-continue

A [Claude Code mod](https://code.claude.com/docs/en/plugins). Part of my wider collection, [claude-mods](https://github.com/icedevil2001/claude-mods).

Hit the 5-hour limit and walk away: this mod waits for the window to reset (plus a margin) and submits "continue" so the session carries on without you.

## Install

```bash
claude plugin marketplace add icedevil2001/auto-continue
claude plugin install auto-continue@auto-continue --scope user
```

Needs a Claude Code build with mods (tested on 2.1.286). Footprint: reads the engine's rate-limit usage, sets one timer, submits one prompt; no network, files or processes.

## Use

- A button **Auto-continue: ON/OFF** appears above the prompt once the 5-hour window is 25% used (or while armed), next to a bar of the window used and the time it resets.
- Or use the slash command: `/auto-continue` (toggle), `/auto-continue on|off|status`.
- When a turn fails on the limit while armed, a toast shows the resume time; typing your own prompt cancels the pending resume.

## Options (config menu)

| Option | Default | |
| --- | --- | --- |
| `threshold` | 25 | % of the 5-hour window at which the button appears |
| `graceMinutes` | 5 | wait after the reset time before sending |
| `message` | continue | the prompt sent on resume |

## Limits

- Only the 5-hour window is handled; the 7-day limit is ignored.
- A limit hit is detected as an API-error turn while the 5-hour window reads >= 95% with a future reset time. If the limit error leaves the usage reading stale, the resume will not arm; see the `ponytail:` note in `hooks/register.tsx`.
- Armed state and the pending timer live in the session; quitting Claude Code drops them.
