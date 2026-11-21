# Counter-Strike 2

Counter-Strike 2, Valve's tactical team shooter on the Source 2 engine.

Native Calagopus egg for the Counter-Strike 2 dedicated server (Steam app `730`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Game mode (a `game_alias`: `competitive`, `casual`, `wingman`, `deathmatch`, ...), start map, player count, VAC, and extra launch parameters or convars (e.g. `+bot_quota 4`).
- Server name, password and RCON password are written to `game/csgo/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel de_mirage`, `mp_restartgame 1`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and Steam server queries) |

## Game Server Login Token

Create a token at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `730` and enter it in Game Server Login Token, so the server keeps its identity and appears in the public server list.

## Files

| Path | Contents |
|------|----------|
| `game/csgo/cfg/server.cfg` | Server settings (created by the installer) |
| `game/csgo/cfg/` | Game mode configs (`gamemode_competitive_server.cfg`, ...) |
| `game/csgo/addons/` | Server plugins (Metamod:Source, CounterStrikeSharp) |

## Stopping

Stopping sends `quit`; the server shuts down within seconds. Counter-Strike 2 keeps no world saves. The egg turns core dumps off so a crash leaves no core files behind.

## Requirements

About 0.75 GB of memory on de_dust2 with bots and no players; plan for 2-4 GB. The installation takes about 68 GB of disk (the server download is the full game), so give the server at least 75 GB.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and plugins:

```
# Ignore all
*
# Except the server configuration and plugins
!game/csgo/cfg/
!game/csgo/addons/
```
