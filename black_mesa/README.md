# Black Mesa

Black Mesa is the Source engine remake of Half-Life. This egg runs its multiplayer deathmatch dedicated server (`dm_` maps, free-for-all or team deathmatch).

Native Calagopus egg for the Source dedicated server (Steam app `346680`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Any map, player count, and extra launch parameters or convars.
- Server name, password and RCON password are written to `bms/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel dm_gasworks`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and Steam server queries) |

## Game Server Login Token

Without a token the server logs in to Steam anonymously and gets a new Steam ID on every start. Create one at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `362890` and enter it in Game Server Login Token.

## Files

| Path | Contents |
|------|----------|
| `bms/cfg/server.cfg` | Server settings (created by the installer) |
| `bms/cfg/mapcycle.txt` | Map rotation |
| `bms/addons/` | Server plugins (Metamod:Source, SourceMod) |

## Stopping

Stopping sends `quit`; the server shuts down within seconds. The game keeps no world saves. The egg turns core dumps off so a crash leaves no core files behind.

## SourceCoop (co-op campaign)

`egg-black-mesa-sourcecoop.yml` runs the single-player campaign in co-op with [SourceCoop](https://github.com/ampreeT/SourceCoop). The installer adds the current stable Metamod:Source and SourceMod (1.12) and the newest SourceCoop release for Black Mesa, and creates the developer's co-op `server.cfg` (team play, no timelimit, home map). Server name, passwords, friendly fire and the starting/home map are set from the panel; Starting Map defaults to `bm_c0a0a`.

SourceCoop's memory extension (Source Scramble) only exists for 32-bit servers, so this egg always runs the 32-bit `srcds_run`. Its plugins (voting, revive, scoring, difficulty, thirdperson and more) are listed with `sm plugins list` in the console. Reinstalling updates Metamod:Source, SourceMod and SourceCoop and keeps `server.cfg`. The co-op server needs about 350 MB of memory with no players and the same 33 GB of disk.

## Requirements

About 280 MB of memory for an empty, hibernating server; more with players. Plan for 1-2 GB. The installation takes about 33 GB of disk, so give the server at least 40 GB: it does not start while the disk limit is exceeded.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and plugins:

```
# Ignore all
*
# Except the server configuration and plugins
!bms/cfg/
!bms/addons/
```
