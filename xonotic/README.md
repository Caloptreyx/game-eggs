# Xonotic

Xonotic is a free and fast-paced arena first-person shooter on the DarkPlaces engine, with deathmatch, CTF, clan arena, duel, freeze tag and more.

Native Calagopus egg for the official Xonotic dedicated server (Linux build from the release archive on dl.xonotic.org). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Installs the current release or a fixed version (Xonotic Version). Reinstall to update; `.xonotic/` (config, maps, logs) is kept.
- Server name, player limit, game type, rcon password and public listing are set from the panel on every start; further `+commands` through Extra Arguments.
- Console commands go straight to the server, e.g. `status`, `gotomap <map>`, `kick <name>`, `ban <name>`, `say <text>`, `endmatch`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 26000   | UDP      | yes (primary allocation; also answers server browser queries) |

## Configuration

The server's own folder is `.xonotic/data/`. Its `server.cfg` is the documented sample config shipped with Xonotic, copied there once by the installer; edit it for the map list (`g_maplist`), time and frag limits, bots, voting and everything else. The panel settings are passed as `+commands`, which run after `server.cfg`, so they win over the file.

Custom maps (`.pk3`) go in `.xonotic/data/`.

## Saves

Xonotic keeps no world state: every match starts fresh. The server log is written to `.xonotic/data/server.log`.

## Requirements

About 0.4 GB of memory on trident with no players; plan for 1 GB. The installation takes about 1.2 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the server folder:

```
# Ignore all
*
# Except the Xonotic config, maps and logs
!.xonotic/
```
