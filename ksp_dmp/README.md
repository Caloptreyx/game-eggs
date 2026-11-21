# Kerbal Space Program: DarkMultiPlayer

[DarkMultiPlayer](https://d-mp.org/) (DMP) is a multiplayer mod for Kerbal Space Program: players fly, dock and build together in a shared universe, each warping time in their own "subspace" and syncing up to each other.

Calagopus egg for the official DMP server from d-mp.org. The server is a .NET Framework program; it runs on Mono, which the installer unpacks from the Debian packages into `.mono/` (the `ghcr.io/caloptreyx/steamcmd:debian` image has no Mono). KSP itself is not needed on the server; players need KSP with the DMP client mod of the same version.

## Capabilities

- Installs the newest DMP server listed on d-mp.org, or the DMP Version set in the panel; reinstall to update. `Config/`, `Universe/`, `Plugins/` and `logs/` are kept.
- Port, server name, player limit, game mode, difficulty, warp mode, modpack mode, whitelist, cheats and MOTD are written to `Config/Settings.txt` on every start. The other settings (asteroids, debris clean-up, screenshots, log level, ...) can be edited there while the server is stopped.
- The console takes server commands: `/help`, `/listclients`, `/kick`, `/ban`, `/admin`, `/whitelist`, `/say`, `/dekessler`, `/nukeksc`, ...
- The panel's stop sends `/exit`, which shuts the server down cleanly. The universe (`Universe/`: vessels, kerbals, scenarios, subspaces) is written to disk as players change it.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Notes |
|------|---------|----------|-------|
| Game | 6702    | TCP/UDP  | primary allocation |

## Mods

Modpack Mode decides how the server shares mods with players: `NONE`, `CKAN` (players get a CKAN file) or `GAMEDATA` (players get the files in `GameData/` on the server). `Config/mod-control.txt` lists the parts and mods players are required or allowed to have.

## Requirements

Under 100 MB of memory with no players (28 MB measured); about 55 MB of disk, mostly Mono. The universe grows with the number of vessels.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the universe and settings:

```
# Ignore all
*
# Except the universe, settings and plugins
!Universe/
!Config/
!Plugins/
```
