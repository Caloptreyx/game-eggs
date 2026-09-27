# Soulmask

Escape a sacrificial ritual, find an ancient mystical mask, rally followers and build your own tribe in this open world survival game.

Native Calagopus egg for the Soulmask dedicated server (Steam app `3017300`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Map selection: Cloud Mist Forest (`Level01_Main`) or Shifting Sands (`DLC_Level01_Main`).
- The console sends commands to the server over its local telnet port, e.g. `help`, `lp` (online players), `qi` (invitation code), `fps`, `say <message>`, `close 300`. The responses appear in the server log on the console.
- Stopping the server saves the world and the players first.
- Optional server clusters (character transfers between servers) and RCON for remote tools.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port    | Default | Protocol | Required |
|---------|---------|----------|----------|
| Game    | 8777    | UDP      | yes (primary allocation) |
| Query   | 27015   | UDP      | yes, for the server list |
| RCON    | 19000   | TCP      | only when an RCON Password is set |
| Cluster | 20000   | TCP      | only on the main server of a cluster; reachable by the client servers, not the public |

The Echo port (18888) is the local telnet port of the panel console. It only listens inside the container and needs no allocation.

## Configuration

Name, passwords, map, max players, game mode and save intervals are panel variables, passed to the server on every start.

The gameplay settings (rates, raids, decay, ...) are stored in `WS/Saved/GameplaySettings/GameXishu.json`. The easiest way to change them is the admin panel in game: open the console, enter `gm key <Admin Password>`, then use the settings menu. The file itself can only be edited while the server is stopped; the settings the server uses are in section `"1"`. The Game Mode variable (`pve` / `pvp`) applies the matching preset on each start.

Other engine settings go in `WS/Saved/Config/LinuxServer/Engine.ini`. Further launch parameters (e.g. `-initbackup`, `-backupinterval=60`, `-GongHuiMaxMember=10`, `-serverpm=1`) can be added under Extra Arguments.

### Server ID

Player characters are stored per Server ID. Changing it after players have joined makes them start over, so settle on one before opening the server.

## Clusters

A cluster lets players move characters between servers, usually one server per map:

1. Main server: Cluster Role `main`, and an extra allocation for the Cluster Port. It holds the player accounts, so it must run for the client servers to work.
2. Client servers: Cluster Role `client`, Cluster Main Address set to `<main server ip>:<cluster port>`.
3. Give every server a different Server ID and use the same Server Password on all of them.
4. Enable "Cross-server Mode" in the gameplay settings of each server players should be able to leave.

Start the main server first and stop it last. Switching an existing world between standalone and cluster changes where player data is stored (`world.db` vs `WS/Saved/Accounts/account.db` on the main server); see the [cluster guide](https://saraserenity.net/soulmask/server_cluster_guide.php) for migrating.

## Saves

The world is saved in `WS/Saved/Worlds/Dedicated/<map>/world.db`; in a cluster the player accounts are in `WS/Saved/Accounts/account.db` on the main server. The server writes `world.db` every Disk Write Interval and on shutdown. `-initbackup` / `-backupinterval=<minutes>` keep backup copies next to it, which the server never deletes.

## Requirements

About 8-10 GB of memory right after startup, growing with players and bases; plan for 16 GB. The installation takes about 3 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your config and saves:

```
# Ignore all
*
# Except the Soulmask saves and config
!WS/Saved/
```
