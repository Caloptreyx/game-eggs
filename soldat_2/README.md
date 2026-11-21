# Soldat 2

Soldat 2 is the sequel to Soldat, the 2D side-scrolling multiplayer shooter: jets, ragdoll physics, bots, team deathmatch, capture the flag and custom levels, rules and modifiers.

Native Calagopus egg for the free Soldat 2 Linux dedicated server from Transhuman Design (`dl.thd.vg`, release or test branch). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Install of the release or test server build (Server Branch); reinstall to update. `autoconfig.ini`, custom levels and configs are kept.
- Server name, passwords, player limit, greeting and the WebSocket RCON port are written to `autoconfig.ini` on every start; everything else in it stays yours.
- WebSocket RCON: connect to `ws://<address>:<RCON port>/rcon` and send the RCON password as the first message; then server commands work (`Info`, `NextMap`, `LoadMap <map> <mode>`, `AddBot`, `RemBot`, `Restart`, ...; see `faq.txt`) and the server log is streamed back.
- The server lists itself in the public games list.
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads no console input; use RCON. The panel's stop sends SIGINT and the server exits at once. Soldat 2 keeps no world state; logs are written to `Logs/`.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 33073   | UDP      | yes (primary allocation) |
| RCON | -       | TCP      | optional: an additional allocation set as RCON Port (0 = off) |

## Configuration

`autoconfig.ini` is seeded by the installer and completed by the server on its first start. Besides the panel's values it holds the map cycle (`[GamesCycle]`, cycles in `Cycles/`), lobby and voting rules, team balance, bots (`FillBotsCount`, `FillBotsDifficulty`), automatic shutdown after hours or matches (`AutoShutdownAfterHours`, `AutoShutdownAfterMatches`) and the built-in patcher (`UseBuiltInPatching`, `RestartAfterPatching`). Edit it while the server is stopped. Custom levels go in `Levels/Custom/`, custom configs in `Configs/Custom/`.

The server shuts itself down after 12 hours or 50 matches by default (`faq.txt` and the setting names describe this; it was not observed in testing); Calagopus then shows it as stopped. Raise those values in `autoconfig.ini` for longer runs.

## Requirements

About 450 MB of memory with no players; plan for 1 GB. The installation takes about 295 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and custom content:

```
# Ignore all
*
# Except the configuration and custom content
!autoconfig.ini
!Levels/Custom/
!Configs/Custom/
```
