# Clone Hero

[Clone Hero](https://clonehero.net/) is a free rhythm game in the style of Guitar Hero, played with guitar controllers, drums or the keyboard, with a huge library of community charts. Online play lets players join a lobby and play songs together.

Native Calagopus egg for the official Clone Hero standalone server (`CloneHero-StandaloneServer.zip` from the [Clone Hero GitHub releases](https://github.com/clonehero-game/releases/releases), Linux x64 build). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Install of the latest release or a fixed tag (Clone Hero Version); reinstall to update. `settings.ini` and `banlist.txt` are kept.
- Server name, password, port and log level are passed on every start.
- The server log is shown in the console (also written to `server.log`). The server reads no console input; the panel's stop sends SIGINT.
- Failed installations are reported to Calagopus as failed, with the reason.

Players need the same Clone Hero version and the charts of the songs they pick; the server keeps no state between sessions.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | any     | UDP      | yes (primary allocation; passed to the server with `-p`) |

## Configuration

`settings.ini` is written by the server on its first start: default game mode, player and spectator limits, whether only the host picks songs, song speed range, tick rate, and optional UDP events and Redis integration. Edit it while the server is stopped. Banned players are listed in `banlist.txt`.

## Requirements

About 20 MB of memory with no players; plan for 128 MB. The installation takes about 25 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings and ban list:

```
# Ignore all
*
# Except the settings and ban list
!settings.ini
!banlist.txt
```
