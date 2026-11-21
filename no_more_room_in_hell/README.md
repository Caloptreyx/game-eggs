# No More Room in Hell

A cooperative first-person zombie survival horror shooter: up to eight players scavenge, share ammunition and complete objectives (`nmo_` maps) or survive waves (`nms_` maps). Set the difficulty with `sv_difficulty` (`casual`, `classic`, `nightmare`) under Extra Arguments.

Native Calagopus egg for the Source dedicated server (Steam app `317670`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Any map, player count, and extra launch parameters or convars.
- Server name, password and RCON password are written to `nmrih/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel nmo_chinatown`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and Steam server queries) |

## Game Server Login Token

Without a token the server logs in to Steam anonymously and gets a new Steam ID on every start. Create one at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `224260` and enter it in Game Server Login Token.

## Files

| Path | Contents |
|------|----------|
| `nmrih/cfg/server.cfg` | Server settings (created by the installer) |
| `nmrih/cfg/mapcycle.txt` | Map rotation |
| `nmrih/addons/` | Server plugins (Metamod:Source, SourceMod) |

## Stopping

Stopping sends `quit`; the server shuts down within seconds. The game keeps no world saves. The egg turns core dumps off so a crash leaves no core files behind.

## Requirements

About 175 MB of memory for an empty server (32-bit build); more with players. Plan for 1-2 GB. The installation takes about 6 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and plugins:

```
# Ignore all
*
# Except the server configuration and plugins
!nmrih/cfg/
!nmrih/addons/
```
