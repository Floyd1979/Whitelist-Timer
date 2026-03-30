# WhitelistTimer

A Fabric server-side mod for Minecraft 1.21.x that automatically removes players from the whitelist after a configurable time period.

## Features

- Add players to the whitelist with a custom expiry duration
- Flexible duration format — seconds, minutes, hours, days, or weeks
- Automatically removes and kicks players when their timer expires
- Optionally reset a player's timer every time they join
- Broadcasts removals to online operators
- Data persists across server restarts
- Lightweight — no extra libraries, uses Gson which ships with Minecraft

## Requirements

- Minecraft 1.21.x
- [Fabric Loader](https://fabricmc.net/use/installer/) `>= 0.15.0`
- [Fabric API](https://modrinth.com/mod/fabric-api)
- Java 21

## Installation

1. Download the latest `.jar` from [Releases](../../releases)
2. Drop it into your server's `mods/` folder alongside Fabric API
3. Start the server — a config file will be generated automatically

## Building from Source

Requires Java 21 and Gradle.

```bash
git clone https://github.com/Floyd1979/Whitelist-Timer
cd whitelist-timer
./gradlew build          # Mac / Linux
gradlew.bat build        # Windows
```

The compiled jar will be at `build/libs/whitelist-timer-1.0.0.jar`.

## Configuration

Located at `config/whitelist-timer.json` on the server.

```json
{
  "defaultDuration": "7d",
  "checkInterval": "10m",
  "resetTimerOnJoin": false,
  "broadcastOnRemove": true,
  "messages": {
    "removed":    "[WhitelistTimer] {player} has been removed from the whitelist (timer expired).",
    "added":      "[WhitelistTimer] {player} added to the whitelist. Expires in {duration}.",
    "timerReset": "[WhitelistTimer] {player}'s whitelist timer has been reset."
  }
}
```

| Field | Default | Description |
|---|---|---|
| `defaultDuration` | `7d` | How long a player stays whitelisted when no duration is specified |
| `checkInterval` | `10m` | How often the server scans for expired entries |
| `resetTimerOnJoin` | `false` | If `true`, a player's timer resets to `defaultDuration` each time they join |
| `broadcastOnRemove` | `true` | Notify online operators when a player is removed |

### Duration Format

Durations are a number followed by a unit letter:

| Unit | Meaning | Example |
|---|---|---|
| `s` | seconds | `90s` |
| `m` | minutes | `30m` |
| `h` | hours | `12h` |
| `d` | days | `7d` |
| `w` | weeks | `2w` |

## Commands

All commands require **operator level 3** (same as `/whitelist`).

| Command | Description |
|---|---|
| `/wltimer add <player> [duration]` | Add a player with a timer. Uses `defaultDuration` if omitted |
| `/wltimer remove <player>` | Remove a player from the timed whitelist immediately |
| `/wltimer check <player>` | Show how much time a player has remaining |
| `/wltimer list` | List all timed entries sorted by soonest to expire |
| `/wltimer reload` | Reload the config file without restarting |

### Examples

```
/wltimer add Steve           # uses defaultDuration from config
/wltimer add Steve 3d        # 3 days
/wltimer add Steve 12h       # 12 hours
/wltimer add Steve 2w        # 2 weeks
/wltimer check Steve         # Steve expires in 2d 18h
/wltimer list                # show all active timers
/wltimer remove Steve        # remove immediately
```

## Data Storage

Player timers are stored in `<world>/whitelist-timer-data.json` and survive server restarts. The standard `/whitelist add` command still works normally — WhitelistTimer only manages entries added via `/wltimer add`.

## License

MIT
