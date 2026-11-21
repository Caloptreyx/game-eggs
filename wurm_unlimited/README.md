# Wurm Unlimited

Wurm Unlimited is the standalone version of the sandbox MMO Wurm Online. Players terraform the land, farm, build houses and villages, craft, trade and fight in a persistent world, here on your own server.

Native Calagopus egg for the Wurm Unlimited dedicated server (Steam app `402370`), installed anonymously through SteamCMD. The launcher brings its own Java runtime.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Name, passwords, player limit, ports, PvP and Epic rules are launch arguments, so panel changes apply on the next start.
- The server log is shown in the console, and console commands work there.
- The panel's stop sends `shutdown`, which saves the world and stops the server.
- Failed installations are reported to Calagopus as failed, with the reason.

The installer copies the shipped Adventure and Creative maps into `worlds/` once; the server runs the world named in World from there, so game updates (which may replace `dist/`) never touch it. To add another map, copy its folder into `worlds/` and set World to its name. Each world folder holds the SQLite databases (`sqlite/`) and the map files.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 3724    | TCP      | yes (primary allocation) |
| Query | 27016   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |

## Requirements

About 650 MB of memory with the Adventure map and no players; plan for 2 GB. The installation takes about 900 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds:

```
# Ignore all
*
# Except the worlds
!worlds/
```
