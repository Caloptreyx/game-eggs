# Urban Terror

Urban Terror is a free team shooter on the Quake III engine, with realistic weapons and fast movement: capture the flag, bomb, team survivor, team deathmatch and more.

Native Calagopus egg for the Urban Terror 4.3 dedicated server (`Quake3-UrT-Ded.x86_64`), installed and updated with the official Urban Terror updater (from the [FrozenSand GitHub releases](https://github.com/FrozenSand/UrTUpdater/releases), since the website only offers it through an interactive page). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Installs the latest Urban Terror version; reinstall to update. `q3ut4/server.cfg`, `q3ut4/mapcycle.txt` and added maps are kept.
- Server name, passwords, player limit and start map are set from the panel on every start; further `+set` options through Extra Arguments.
- Console commands go straight to the server, e.g. `status`, `map <map>`, `kick <name>`, `say <text>`. Stopping the server sends `quit`.
- The server sends heartbeats to the Urban Terror master servers.
- Failed installations are reported to Calagopus as failed, with the reason.

Urban Terror keeps no world state.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27960   | UDP      | yes (primary allocation; also answers server browser queries) |

## Configuration

`q3ut4/server.cfg` (copied once from the shipped `server_example.cfg`) runs first on every start; the panel settings are passed afterwards and win. Edit it for the game type (`g_gametype`: 4 team survivor, 7 capture the flag, 8 bomb, ...), limits, auth settings and votes. The map rotation is `q3ut4/mapcycle.txt`; custom maps (`.pk3`) go in `q3ut4/`.

## Requirements

About 160 MB of memory with no players; plan for 512 MB. The installation takes about 1.4 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and custom maps:

```
# Ignore all
*
# Except the configuration and map cycle
!q3ut4/*.cfg
!q3ut4/mapcycle.txt
```
