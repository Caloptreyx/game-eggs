# Veloren

Veloren is an open-world, open-source multiplayer voxel RPG inspired by Cube World, Legend of Zelda: Breath of the Wild, Dwarf Fortress and Minecraft: explore a procedurally generated world, fight, craft, and visit towns simulated in the background.

Native Calagopus egg for the official Veloren server (`veloren-server-cli`, weekly or nightly Linux build from download.veloren.net). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Installs the weekly (recommended) or nightly build. Reinstall to update; `userdata/` is kept.
- Server name, message of the day, player limit, PvP/PvE, authentication and the ports are set from the panel on every start.
- Console commands go straight to the server, e.g. `list-players`, `send-global-msg <text>`, `admin add <username> admin`, `disconnect-all-clients`, `help`.
- Stopping the server sends `shutdown graceful 5`: players are warned, the world simulation is saved, and the server exits.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 14004   | TCP      | yes (primary allocation) |
| Query | 14006   | UDP      | additional allocation set as Query Port (server information for the server browser) |

The server's web API (port 14005) only listens on 127.0.0.1 inside the container.

## Configuration

The settings are RON files in `userdata/server/server_config/`. `settings.ron` and `description.ron` are created by the installer (with a random world seed); on every start the panel writes the Server Name, Max Players, Battle Mode and ports into `settings.ron` and the Message of the Day into `description.ron`. Everything else (world seed, day length, view distance, group size, ...) is edited in those files while the server is stopped. Admins, bans and the whitelist are in `admins.ron`, `banlist.ron` and `whitelist.ron` next to them.

With Authentication on (the default) players log in with a Veloren account; turn it off only for private servers.

## Admins

Add an admin from the console with `admin add <username> admin` (or `moderator`); the list is kept in `admins.ron`.

## Saves

Characters are stored in `userdata/server/saves/db.sqlite`, the simulated world (towns, NPCs, resources) in `userdata/server/rtsim/data.dat`; the terrain is generated from the world seed. The world simulation is saved periodically and when the server is stopped from the panel.

## Requirements

About 0.6-0.7 GB of memory with a fresh world and no players; plan for 2-4 GB, more with players spread over the world. The installation takes about 530 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings, characters and world:

```
# Ignore all
*
# Except the Veloren settings and saves
!userdata/
```
