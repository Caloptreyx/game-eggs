# Colony Survival

Colony Survival is a voxel colony builder and tower defense game: found a colony, assign colonists to jobs, research and automate production, and defend against monsters that attack at night, alone or together.

Native Calagopus egg for the Colony Survival dedicated server (Steam app `748090`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- World, server name, player limit, password, network type and ports are passed on every start; a missing world is created on start.
- With the SteamOnline network type the server registers with the Colony Survival master server and is listed in game.
- The server log is shown in the console. The panel's stop sends SIGINT; the server saves the world and exits.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 27016   | UDP      | yes (primary allocation) |
| Steam | 27005   | UDP      | yes, additional allocation set as Steam Port |

## Configuration and saves

Worlds are in `gamedata/savegames/<world>/` (`world.sqlite3`, `chunks.sqlite3`, `worldconfig.json`); the server's `blacklist.txt` and `whitelist.txt` are in `gamedata/savegames/`. Server settings that are not panel variables can be passed as `+server.<setting> <value>` in Extra Arguments; mods go into `gamedata/mods/` (see `gamedata/help/` for the modding and hosting notes shipped with the server).

## Requirements

About 270 MB of memory for a fresh world with no players; plan for 2 GB, more for large colonies. The installation takes about 560 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and mods:

```
# Ignore all
*
# Except worlds, lists and mods
!gamedata/savegames/
!gamedata/mods/
```
