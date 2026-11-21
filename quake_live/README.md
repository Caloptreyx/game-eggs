# Quake Live

Quake Live is id Software's free-to-play take on Quake III Arena: duel, free for all, clan arena, capture the flag, team deathmatch, freeze tag, race and more.

Native Calagopus egg for the Quake Live dedicated server (Steam app `349090`), installed anonymously through SteamCMD. It uses the **native Linux build** (`qzeroded.x64`) and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, password, player limit, server type, start map and factory (game type) are set from the panel on every start; further `+set` options through Extra Arguments.
- Console commands go straight to the server, e.g. `status`, `map <map> <factory>`, `kick <id>`, `say <text>`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27960   | UDP      | yes (primary allocation; also answers Steam server queries) |

## Configuration

The settings files are in `baseq3/`: `server.cfg` (created by the installer, run first on every start; the panel settings override it), the map pools (`mappool.txt`, `mappool_<factory>.txt`), `access.txt` (admins and bans by SteamID64) and `workshop.txt` (Steam Workshop items to download).

The server starts on Map with the chosen Factory, then picks its maps from the factory's map pool (`mappool_ffa.txt`, `mappool_ca.txt`, `mappool_ctf.txt`, `mappool_duel.txt`, `mappool_race.txt`, `mappool_tdm.txt`).

## Admins

Add admins to `baseq3/access.txt`, one `SteamID64|admin` (or `mod`, `ban`) per line, while the server is stopped.

## Saves

Quake Live keeps no world state: every match starts fresh.

## Requirements

About 50 MB of memory with no players; plan for 512 MB. The installation takes about 1.4 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Quake Live configuration
!baseq3/*.cfg
!baseq3/*.txt
```
