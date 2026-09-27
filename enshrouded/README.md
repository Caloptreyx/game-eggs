# Enshrouded

Survival action RPG with voxel building: fight the Shroud, explore a vast world and build your base, solo or with up to 16 players.

Native Calagopus egg. The dedicated server (Steam app `2278520`) only exists for Windows. It is installed anonymously through SteamCMD and runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- The server log is streamed to the console. The server reads no console input.
- Name, port, max players, difficulty preset and the user group passwords are written to `enshrouded_server.json` on every start.
- Stopping the server saves the world first.
- Failed installations are reported to Calagopus as failed, with the reason.

## First start

The server creates `enshrouded_server.json` on its first start, with random passwords for the user groups. The panel settings are applied from the next start on, so restart the server once after the first start.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Query | 15637   | UDP      | yes (primary allocation); any port works |

The server has no separate game port any more; players connect through the query port. It also opens a random UDP port for Steam and a local TCP port on 127.0.0.1, neither of which needs an allocation.

## Configuration

All settings are in `enshrouded_server.json`. The server rewrites it on each start, so edit it only while the server is stopped.

- **Difficulty Preset:** `Default`, `Relaxed`, `Hard` or `Survival`. `Custom` uses the individual values under `gameSettings` in the file; see `enshrouded_server_readme.txt` for the list and valid ranges.
- **User groups:** players join with the password of a group and get its permissions:

| Group   | Build in bases | Chests in bases | Change the world | Kick and ban |
|---------|----------------|-----------------|------------------|--------------|
| Admin   | yes            | yes             | yes              | yes          |
| Friend  | yes            | yes             | yes              | no           |
| Guest   | no             | no              | yes              | no           |
| Visitor | no             | no              | no               | no           |

Each group has a panel variable for its password. An empty variable keeps the password in the file, which starts out random, so a group nobody knows the password of is effectively closed. Permissions, `reservedSlots`, extra groups, tags, voice and text chat are edited in the file.

The four groups are only created with a new `enshrouded_server.json`. A file without `userGroups` (for example from an old version) gets a single `Default` group instead, which the password variables do not cover; delete the file while the server is stopped to get the four groups.

## Saves

The world is saved in `savegame/`: `3ad85aea` plus its `-index` file, with older copies as `3ad85aea-1`, `3ad85aea-2`, ... The server saves when it stops. Logs are in `logs/`, earlier ones in `logs/backup/`.

## Stopping

The panel's stop sends SIGINT, which the startup command passes on to the server: it saves, logs `Close Log file` and exits within a second or two. Killing the server skips this save.

## Requirements

An empty server uses about 0.8-1 GB of memory; more with players and bases. The installation takes about 8.5 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your config and saves:

```
# Ignore all
*
# Except the Enshrouded saves and config
!savegame/
!enshrouded_server.json
```
