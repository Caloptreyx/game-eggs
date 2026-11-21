# Modiverse

Modiverse is a free sandbox for creating and playing mods such as TTT, Deathrun, FortWars and roleplay. Players build with props, lights, thrusters and wheels, and servers run game modes and maps from the Steam Workshop.

Native Calagopus egg for the Modiverse dedicated server (Steam app `1549820`), installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, ports and player limit are launch arguments; the RCON password is written to `ServerConfiguration.json` on every start.
- The server downloads the game modes, maps and assets of its map cycle from the Steam Workshop on start (`-InitUGCs`), into `KJMod/Binaries/Linux/ServerData/`.
- The Unreal log is shown in the console; the developers note that the beta build logs a lot of debug output. The server reads no console input.
- The panel's stop sends SIGINT and the server exits. It keeps no world state.
- Failed installations are reported to Calagopus as failed, with the reason.

The map cycle is configured in `KJMod/Binaries/Linux/ServerData/ServerConfiguration.json`. The installer seeds it with the developer's example, a Deathrun mode and map. Each entry takes the Workshop ids of the game mode, the map and any extra assets, plus a duration in seconds:

```json
{"gameMode": "2285067974", "comment": "Deathrun, Deathrun Example Map", "map": "2286680373", "assets": ["2229481041"], "duration": 1800}
```

The file also holds the admin `pin`, the MOTD URL and delay, and the `rconPassword`.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |

## Requirements

About 220 MB of memory with the example map cycle and no players; plan for 2 GB. The installation takes about 650 MB of disk, plus the Workshop content of the map cycle (about 300 MB for the example).

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the server configuration:

```
# Ignore all
*
# Except the map cycle and settings
!KJMod/Binaries/Linux/ServerData/ServerConfiguration.json
```
