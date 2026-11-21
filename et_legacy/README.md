# ET: Legacy

ET: Legacy is the open source continuation of Wolfenstein: Enemy Territory, the free team-based objective shooter: Axis against Allies with five classes on maps such as Oasis, Goldrush and Radar.

Native Calagopus egg for the official ET: Legacy dedicated server (`etlded`, 64-bit Linux release from etlegacy.com) with the original Enemy Territory game data (`pak0`-`pak2`, from the ET: Legacy mirror). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Installs the current ET: Legacy release; reinstall to update. `etmain/etl_server.cfg`, added maps and the game data are kept.
- Server name, passwords, player limit, start map and Omni-bot bots are set from the panel on every start; further `+set` options through Extra Arguments.
- Console commands go straight to the server, e.g. `status`, `map <map>`, `kick <name>`, `say <text>`. Stopping the server sends `quit`.
- WolfAdmin (shipped with ET: Legacy) adds in-game admin commands.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27960   | UDP      | yes (primary allocation; also answers server browser queries) |

## Configuration

`etmain/etl_server.cfg` (the documented server config shipped with ET: Legacy) runs first on every start; the panel settings are passed afterwards and win. The map rotations are in the `*cycle.cfg` files next to it (`objectivecycle.cfg`, `campaigncycle.cfg`, `lmscycle.cfg`, `mapvotecycle.cfg`); custom maps (`.pk3`) go in `etmain/`.

Max Players includes the private slots set in `etl_server.cfg` (`sv_privateclients`, 4 by default).

## Saves

ET: Legacy keeps no world state: every match starts fresh. Player statistics and WolfAdmin data are kept in `etl.db`.

## Requirements

About 140 MB of memory with no players; plan for 512 MB. The installation (server and game data) takes about 370 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and database:

```
# Ignore all
*
# Except the ET: Legacy configs and database
!etmain/*.cfg
!etl.db
```
