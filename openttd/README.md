# OpenTTD

OpenTTD is an open source simulation game based on Transport Tycoon Deluxe: build road, rail, sea and air networks, transport passengers and cargo, and compete with other companies.

Native Calagopus egg for the official OpenTTD dedicated server (Linux generic build from cdn.openttd.org) with the free OpenGFX graphics set. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Installs the current stable release, the newest testing release or a fixed version (OpenTTD Version). Reinstall to update or change versions; `openttd.cfg`, `save/` and downloaded content are kept.
- Server name, passwords, visibility, client and company limits, ports and the new-map settings (size, starting year) are written to `openttd.cfg` on every start.
- The game is saved on stop and loaded again on the next start.
- Console commands go straight to the server, e.g. `help`, `server_info`, `clients`, `companies`, `say "<text>"`, `kick <client id>`, `save <name>`, `newgame`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol  | Required |
|-------|---------|-----------|----------|
| Game  | 3979    | TCP + UDP | yes (primary allocation) |
| Admin | 3977    | TCP       | only for admin tools (Admin Password set); additional allocation set as Admin Port |

Public and invite-only servers register with OpenTTD's Game Coordinator, which also lets players behind NAT connect via its STUN/TURN services; the server prints its invite code on start.

## Configuration

`openttd.cfg` is created by the installer with the settings the panel manages; the server runs with `-x`, so it never rewrites the file. Add any other setting (e.g. `[difficulty]`, `[economy]`, `[game_creation] landscape`, `[network] autoclean_companies`) to the file while the server is stopped; the server's console command `setting <name> <value>` changes settings of the running game.

Map Width, Map Height and Starting Year only apply when a new map is generated: on the first start, or after deleting `save/autosave/exit.sav`. To load another save, stop the server and copy it to `save/autosave/exit.sav`.

## Saves

The game is saved to `save/autosave/exit.sav` when the server is stopped from the panel (the console command `exit` with `autosave_on_exit`), and loaded from there on the next start. Regular autosaves go to `save/autosave/`, manual saves (`save <name>`) to `save/`.

## Requirements

About 10-15 MB of memory with a new 256×256 map and no players; more for large maps and many vehicles. Plan for 256-512 MB. The installation takes about 90 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the saves and config:

```
# Ignore all
*
# Except the OpenTTD saves and config
!save/
!openttd.cfg
```
