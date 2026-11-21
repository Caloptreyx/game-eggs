# Teeworlds

Teeworlds is a free online multiplayer 2D shooter with grappling hooks and cute tees: deathmatch, team deathmatch, capture the flag, last man standing and last team standing.

Native Calagopus egg for the official Teeworlds server (Linux build from the GitHub releases of teeworlds/teeworlds). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Installs the latest release or a fixed release tag (Teeworlds Version). Reinstall to update; settings and maps in `teeworlds-data/` are kept.
- Server name, passwords, game type, start map, player limit and master server registration are set from the panel on every start; any other console command through Extra Arguments.
- The panel's stop sends SIGINT; the server exits cleanly.
- Failed installations are reported to Calagopus as failed, with the reason.

The console shows the server log. The server reads no console input; admins log in to the in-game remote console with the RCON Password (when it is empty, the server prints a random one on every start).

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 8303    | UDP      | yes (primary allocation) |

With Public on, the server registers with the Teeworlds master servers, which also check that it is reachable ("no firewall/nat problems detected" in the console).

## Configuration

The server's own folder is `teeworlds-data/teeworlds/`. Its `autoexec.cfg` (created by the installer with a deathmatch map rotation) runs on every start, before the panel settings, which therefore win. Add any other setting there, one console command per line (e.g. `sv_maprotation`, `sv_scorelimit`, `sv_timelimit`, `sv_teambalance_time`). Extra Arguments takes further commands separated by `;`.

Custom maps go in `teeworlds-data/teeworlds/maps/` (the built-in maps, `dm1`-`dm9`, `ctf1`-`ctf8`, ..., are in `data/maps/`); set Map to the file name without `.map`. The start map has to fit the game type (e.g. a `ctf` map for capture the flag).

## Saves

Teeworlds keeps no world state: every match starts fresh. `teeworlds-data/teeworlds/` holds the config, maps and recorded demos.

## Requirements

About 11 MB of memory with no players; plan for 128 MB. The installation takes about 12 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the server folder:

```
# Ignore all
*
# Except the Teeworlds settings and maps
!teeworlds-data/
```
