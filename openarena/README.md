# OpenArena

OpenArena is a free and open source arena shooter built on the Quake III Arena engine: fast deathmatch, team deathmatch, capture the flag, elimination and more, with its own maps, models and bots.

Native Calagopus egg for the OpenArena 0.8.8 dedicated server (`oa_ded.x86_64`, the final release, downloaded from SourceForge with the archived openarena.ws mirror as fallback). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Server name, passwords, player limit and start map are set from the panel on every start; further `+set` options through Extra Arguments.
- The installer creates `baseoa/server.cfg` once with the game type, limits, bot settings and a deathmatch map rotation (aggressor, oa_dm1-oa_dm5).
- Console commands go straight to the server, e.g. `status`, `map <map>`, `vstr d1`, `addbot <name>`, `kick <name>`, `say <text>`. Stopping the server sends `quit`.
- The server announces itself to the dpmaster server list.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27960   | UDP      | yes (primary allocation; also answers server browser queries) |

## Configuration

`baseoa/server.cfg` runs first on every start; the panel settings are passed afterwards and win. Edit it for the game type (`g_gametype`: 0 deathmatch, 1 tournament, 3 team deathmatch, 4 capture the flag, 8 elimination, ...), frag, capture and time limits, bots (`bot_minplayers`, `g_spSkill`) and the map rotation (`d1`-`d6`; each entry loads a map and points `nextmap` to the next one). Custom maps (`.pk3`) go in `baseoa/`.

## Saves

OpenArena keeps no world state: every match starts fresh.

## Requirements

About 25 MB of memory with no players; plan for 256 MB. The installation takes about 400 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and custom maps:

```
# Ignore all
*
# Except the configuration and added maps
!baseoa/*.cfg
!baseoa/*.pk3
baseoa/pak*.pk3
```
