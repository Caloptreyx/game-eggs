# Plains of Pain

Plains of Pain is a top-down open-world survival game set in a post-apocalyptic desert. Players scavenge, craft, drive, build bases and survive raiders and the environment, alone or together.

Native Calagopus egg for the Plains of Pain dedicated server (Steam app `2227360`), installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, password, player limit, ports and the world settings (World ID, Map ID, Seed, Difficulty, World Size) are written to `configs/server.json` on every start. The file is created once from the shipped `configs/default.json`; other keys you add stay.
- The Unity log is shown in the console. The server reads no console input.
- The panel's stop sends SIGINT; the server saves the world and exits.
- Failed installations are reported to Calagopus as failed, with the reason.

Worlds and player profiles are saved in `data/` (`userdir` in `configs/server.json`), with the server log in `data/server.log`. Map, seed, difficulty and size only apply to a new world; change World ID to start one. `ttr` (default 28800) is a timed server restart in seconds, as shipped.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27016   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |

## Requirements

About 1.2 GB of memory with an empty world; plan for 3 GB. The installation takes about 1 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and settings:

```
# Ignore all
*
# Except the saves and the server config
!data/
!configs/server.json
```
