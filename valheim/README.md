# Valheim

A brutal exploration and survival game for 1-10 players, set in a procedurally-generated purgatory inspired by viking culture. Battle, build, and conquer your way to a saga worthy of Odin's patronage!

Native Calagopus egg for the Valheim dedicated server (Steam app `896660`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch (e.g. `public-test`).
- Steam backend, or the crossplay backend (PlayFab) so players on every platform can join by IP, join code or server list.
- Server name, password, world, visibility, save interval and backups are set from the panel; world modifiers through Extra Arguments.
- Stopping the server saves the world first.
- Failed installations are reported to Calagopus as failed, with the reason.

The server has no console input. The console shows the server log; admin commands (`kick`, `ban`, `unban`, `banned`, `save`, ...) are entered in the in-game console (F5) by a player on the admin list.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 2456    | UDP      | yes (primary allocation) |
| Game | 2457    | UDP      | yes, primary + 1 (Steam server queries) |

Allocate the port following the primary allocation as an additional allocation. With Crossplay on, the server connects out to a PlayFab relay and does not listen on the Game port; players still connect with the server's IP and port, a join code or the server list. Only the Steam backend can be joined by a local or loopback IP.

## Configuration

Name, password, world, visibility (Public), backend (Crossplay), save interval and backups are panel variables, passed to the server on every start.

The Server Password is required: at least 5 characters, and it must not be part of the Server Name (case-insensitive). The server refuses to start otherwise and says why in the console.

World modifiers go under Extra Arguments and apply to the loaded world:

- `-preset <name>`: `Normal`, `Casual`, `Easy`, `Hard`, `Hardcore`, `Immersive`, `Hammer`. Overwrites all other modifiers, so put it first.
- `-modifier <modifier> <value>`: `combat` (`veryeasy`, `easy`, `hard`, `veryhard`), `deathpenalty` (`casual`, `veryeasy`, `easy`, `hard`, `hardcore`), `resources` (`muchless`, `less`, `more`, `muchmore`, `most`), `raids` (`none`, `muchless`, `less`, `more`, `muchmore`), `portals` (`casual`, `hard`, `veryhard`).
- `-setkey <key>`: `nobuildcost`, `playerevents`, `passivemobs`, `nomap`.
- `-backupshort <seconds>` / `-backuplong <seconds>`: backup intervals (default 7200 / 43200).
- `-instanceid <id>`: needed when several crossplay servers share a port and machine, so each gets its own PlayFab id.

Example: `-preset hard -modifier raids none -setkey nomap`

## Admins, bans and permitted players

`saves/adminlist.txt`, `saves/bannedlist.txt` and `saves/permittedlist.txt` hold one Platform User ID per line, e.g. `Steam_76561198000000000` (case sensitive). The IDs are shown in the server log when a player connects and in game in the F2 panel. As soon as `permittedlist.txt` contains an ID, everyone not on it is refused.

## Saves

Worlds are stored in `saves/worlds_local/`, next to the permission lists. Current server versions keep each world in its own folder, `saves/worlds_local/<World Name>/` (`_main.<n>.db2`, `_main.<n>.fwl2` and chunk files, `<n>` counting up with each save). The server saves every Save Interval and on shutdown; automatic backups are kept next to the world files. To use an existing world, stop the server, upload it to `saves/worlds_local/` (a `<name>.db` + `<name>.fwl` pair or a world folder) and set World Name to its name.

## Requirements

About 1.3 GB of memory with a new world and no players, more with players and explored area; plan for 4 GB. The installation takes about 2.3 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and permission lists:

```
# Ignore all
*
# Except the Valheim saves
!saves/
```
