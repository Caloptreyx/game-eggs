# Soldat

Soldat is a fast 2D side-scrolling multiplayer shooter inspired by Liero, Worms and Counter-Strike: deathmatch, capture the flag, infiltration and more, with jets, grenades and a large arsenal.

Native Calagopus egg for the Soldat dedicated server (Steam app `638500`, server version 2.8.2 / game 1.7.1), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Port, server name, passwords and player limit are written to `soldat.ini` on every start; everything else in it stays yours.
- The server registers with the Soldat lobby and with Steam.
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads no console input. It is administered remotely: Soldat Admin (or any TCP client) connects to the game port and logs in with the admin password; then commands like `/map <map>`, `/kick <name>`, `/addbot <name>` work. The panel's stop sends SIGINT and the server exits at once. Soldat keeps no world state.

## Server Ports

| Port        | Default | Protocol | Required |
|-------------|---------|----------|----------|
| Game        | 23073   | UDP      | yes (primary allocation) |
| Admin       | 23073   | TCP      | same port as the game, TCP (remote admin) |
| File server | 23083   | TCP      | always the game port + 10 (map and mod downloads for players); allocate it too |

## Configuration

| File | Contents |
|------|----------|
| `soldat.ini` | Game settings (`[GAME]`: game style, limits, weapons, bots, ...) and network settings (`[NETWORK]`: greeting messages, ping limits, lobby registration, ...) |
| `server.ini` | Server extras: team-kill punishment, admin connections, scripting |
| `mapslist.txt` | Map rotation, one map per line |
| `remote.txt` | IP addresses of players with admin rights in game |

Edit them while the server is stopped; the panel's values in `soldat.ini` are written on every start.

## Requirements

About 16 MB of memory with no players; plan for 128 MB. The installation takes about 370 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the configuration
!*.ini
!mapslist.txt
!remote.txt
```
