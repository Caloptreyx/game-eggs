# Necesse

Necesse is a top-down sandbox survival game: explore an endless procedurally generated world, gather resources, build a settlement, recruit settlers to work for you and fight your way through bosses and dungeons, alone or with friends.

Native Calagopus egg for the Necesse dedicated server (Steam app `1169370`), installed anonymously through SteamCMD. It uses the **native Linux build** with the Java runtime bundled in the server files and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- World, password, slots, owner, message of the day and pause-when-empty are set from the panel on every start.
- The console passes commands straight to the server, without the leading `/`, e.g. `help`, `players`, `save`, `say <message>`, `kick <player>`, `ban <name>`, `permissions set <name> admin`, `difficulty <difficulty>`, `settings list`.
- Stopping the server sends `stop`, which saves the world before the server exits.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 14159   | UDP      | yes (primary allocation) |

No other allocation is needed. The server also opens a LAN discovery socket on a random port, which is only used on a local network.

## Configuration

World, password, slots, owner, message of the day and Pause When Empty are passed to the server as launch options on every start; they apply to that run and take precedence over `cfg/server.cfg`. The other server settings (`maxClientLatencySeconds`, `strictServerAuthority`, `unloadLevelsCooldown`, `droppedItemsLifeMinutes`, settlement limits, ...) are in `cfg/server.cfg`, written by the server on the first start; edit it while the server is stopped.

World rules (difficulty, death penalty, raid frequency, hunger, PvP, ...) are stored in each world and changed from the console or in game by an admin: `difficulty <casual/adventure/classic/hard/brutal>`, `deathpenalty <list/penalty>`, `raids <list/frequency>`, `settings list`.

Extra Arguments takes further launch options, e.g. `-language de`, `-zipsaves 0` (store new saves uncompressed), `-ignoreseasons`, `-logging 0`.

The Java heap may use up to 75% of the server's memory limit, so the heap follows the memory you give the server.

## Admins

Set Owner Name to your character name: whoever joins with that name gets owner permissions. Otherwise give permissions from the console with `permissions set <name> <user/moderator/admin/owner>`. Permissions and bans are kept in `cfg/`.

## Saves

Worlds are saved in `saves/worlds/<World Name>.zip`, player characters inside the world. The server saves on `save` and `stop`. To use an existing world, stop the server, upload the `.zip` (or the world folder) to `saves/worlds/` and set World Name to its name (without `.zip`). Server logs go to `logs/` and `latest-server-log.txt`.

## Requirements

About 0.7 GB of memory with a new world and no players (4 GB limit, so a heap of up to 3 GB); more with players and a larger explored world. Plan for 2-4 GB. The installation takes about 600 MB of disk; a new world about 20 KB, growing as it is explored.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and config:

```
# Ignore all
*
# Except the Necesse worlds and config
!saves/
!cfg/
```
