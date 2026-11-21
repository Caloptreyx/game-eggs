# Half-Life 2: Deathmatch

Fast-paced multiplayer deathmatch with Half-Life 2's weapons and the gravity gun, free-for-all or team deathmatch (`mp_teamplay 1`).

Native Calagopus egg for the Source dedicated server (Steam app `232370`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Any map, player count, and extra launch parameters or convars.
- Server name, password and RCON password are written to `hl2mp/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel dm_overwatch`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and Steam server queries) |

## Game Server Login Token

Without a token the server logs in to Steam anonymously and gets a new Steam ID on every start. Create one at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `320` and enter it in Game Server Login Token.

## Files

| Path | Contents |
|------|----------|
| `hl2mp/cfg/server.cfg` | Server settings (created by the installer) |
| `hl2mp/cfg/mapcycle.txt` | Map rotation |
| `hl2mp/addons/` | Server plugins (Metamod:Source, SourceMod) |

## Stopping

Stopping sends `quit`; the server shuts down within seconds. The game keeps no world saves. The egg turns core dumps off so a crash leaves no core files behind.

## Requirements

About 85 MB of memory for an empty, hibernating server (32-bit build); more with players. Plan for 1 GB. The installation takes about 1.2 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and plugins:

```
# Ignore all
*
# Except the server configuration and plugins
!hl2mp/cfg/
!hl2mp/addons/
```
