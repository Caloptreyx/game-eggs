# Team Fortress 2 Classified

Team Fortress 2 Classified (formerly Team Fortress 2 Classic) is a free Source mod built on Team Fortress 2, with four-team modes, VIP, domination and other game modes and reworked weapons.

Native Calagopus egg for the Team Fortress 2 Classified dedicated server (Steam app `3557020`), installed anonymously through SteamCMD together with the Team Fortress 2 dedicated server (app `232250`), whose content the mod loads. It runs the 64-bit Linux server on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update): the image updates the mod, the startup command updates the Team Fortress 2 dedicated server in `tf2/`.
- Any map, player count, and extra launch parameters or convars.
- Server name, password and RCON password are written to `tf2classified/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel pl_badwater`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and queries) |

## Game Server Login Token

Servers without a token only show up in the server list for LAN. Create one at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `3545060` (Team Fortress 2 Classified) and enter it in Game Server Login Token.

## Files

| Path | Contents |
|------|----------|
| `tf2classified/cfg/server.cfg` | Server settings |
| `tf2classified/cfg/mapcycle.txt` | Map rotation (falls back to `mapcycle_default.txt`) |
| `tf2classified/cfg/motd.txt` | Message of the day (falls back to `motd_default.txt`) |
| `tf2/` | The Team Fortress 2 dedicated server the mod needs; do not remove it |

## Stopping

Stopping sends `quit`; the server shuts down in a couple of seconds, and the egg turns core dumps off so a crash at the end of the shutdown leaves no core files behind. The game keeps no world saves.

## Requirements

- About 22 GB of disk: 7 GB for the mod and 15 GB for the Team Fortress 2 dedicated server.
- About 400 MB of RAM for an empty server; plan for 2 GB.

## Sample `.pteroignore`

```
steamcmd/
steamapps/
tf2/
bin/
```
