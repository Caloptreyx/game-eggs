# Open Fortress

Open Fortress is a free Source mod built on Team Fortress 2, with deathmatch, the mercenary class, arsenal, gun game and other game modes, new weapons and maps.

Native Calagopus egg for an Open Fortress dedicated server. It installs anonymously through SteamCMD the Source SDK Base 2013 Multiplayer dedicated server (Steam app `244310`) and the Team Fortress 2 dedicated server (app `232250`), whose content the mod loads, plus the Open Fortress release from adastral (the source of its launcher, beans). It runs the 32-bit Linux server on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update): the image updates the Source SDK Base, the startup command updates the Team Fortress 2 dedicated server in `tf2/`.
- Open Fortress itself is updated by reinstalling: the installer downloads the newest release listed at `of-proxy.kate.pet/versions.json` (about 3.3 GB, unpacked to about 7 GB) when it is newer than the installed one (`open_fortress/.of-release`). `open_fortress/cfg/server.cfg` and files that are not part of the release (custom content, extra maps) are kept.
- Any map, player count, and extra launch parameters or convars.
- Server name, password and RCON password are written to `open_fortress/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel dm_lumberyard`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

The current Open Fortress release is built for the 2021 Source SDK Base engine; on the February 2025 public build it fails with "Failed to apply server patches!" and crashes. The egg therefore installs and updates the `previous2021` branch of app `244310`. The installer also points `open_fortress/gameinfo.txt` at `tf2/` (the shipped file expects a Windows Team Fortress 2 client next to the SDK) and adds the file names the dedicated engine looks for: `bin/engine.so` for `bin/engine_srv.so` and so on, and `open_fortress/bin/server_srv.so` for the mod's `server.so` (without it srcds crashes right after mounting the content).

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and queries) |

## Game Server Login Token

Optional. Without one the server gets an anonymous Steam ID on every start; with a token (from [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers), app ID `243750`) it keeps a persistent one.

## Files

| Path | Contents |
|------|----------|
| `open_fortress/cfg/server.cfg` | Server settings |
| `open_fortress/cfg/mapcycle.txt` | Map rotation (falls back to `mapcycle_default.txt`) |
| `open_fortress/cfg/motd.txt` | Message of the day (falls back to `motd_default.txt`) |
| `tf2/` | The Team Fortress 2 dedicated server the mod needs; do not remove it |

## Stopping

Stopping sends `quit`; the server shuts down in a couple of seconds, and the egg turns core dumps off so a crash at the end of the shutdown leaves no core files behind. The game keeps no world saves.

## Requirements

- About 23 GB of disk: 15 GB for the Team Fortress 2 dedicated server, 7 GB for Open Fortress and 1 GB for the Source SDK Base. The installation downloads about 3.3 GB for Open Fortress plus the Steam content.
- About 400 MB of RAM for an empty server; plan for 2 GB.

## Sample `.pteroignore`

```
steamcmd/
steamapps/
tf2/
bin/
```
