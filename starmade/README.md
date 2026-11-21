# StarMade

StarMade is a free voxel space sandbox. Players build ships and stations block by block, mine asteroids and planets, trade, form factions and fight in a procedurally generated universe.

Calagopus egg for the StarMade dedicated server, installed and updated with the official [StarMade starter](http://files.star-made.org/StarMade-Starter.jar). Current StarMade builds need Java 21 and must be launched with `-javaagent:StarMade.jar` and two `--add-opens` flags, which the startup command does.

## Capabilities

- The starter updates StarMade on every start when Auto Update is on (with `-nobackup`; the starter's own database backups would otherwise pile up).
- Player limit, server list name, description and announcement, the super admin password, autosave interval and world name are written to `StarMade/server.cfg` on every start; everything else in it stays yours. The installer seeds the managed keys and the server adds all other settings, with comments, on its first start.
- Console commands work in the panel console (`/help`, `/save`, `/kick`, ...).
- The panel's stop sends `/shutdown 0`, which saves the universe and stops the server.
- Failed installations are reported to Calagopus as failed, with the reason.

The game is installed in `StarMade/`; the universe is saved in `StarMade/server-database/<World Name>/`. Change World Name to start a new universe.

To be listed in the StarMade server list, set Announce To Server List to `true` and Announced Host to the server's public address.

## Server Ports

| Port | Default | Protocol |
|------|---------|----------|
| Game | 4242    | TCP (UDP only with `USE_UDP = true`) |

## Requirements

About 1.1 GB of memory with a new universe and no players; plan for 4 GB. The installation takes about 1.4 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the universe, blueprints and settings:

```
# Ignore all
*
# Except the universe, blueprints and settings
!StarMade/server-database/
!StarMade/blueprints/
!StarMade/server.cfg
!StarMade/*.txt
```
