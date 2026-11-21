# Avorion

A procedural co-op space sandbox where players can build their own space ships out of dynamically scalable blocks. Fight epic space battles, explore, mine, trade, wage wars and build your own empire to save your galaxy from being torn apart by an unknown enemy.

Native Calagopus egg for the Avorion dedicated server (Steam app `565060`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- Server name, password, admin, galaxy, player limit, difficulty, scenario, seed, server list visibility and save interval are set from the panel; any other server parameter through Extra Arguments.
- Console input: server commands such as `/save`, `/stop`, `/kick`, `/ban`, `/say` or `/help` are typed in the panel console.
- Stopping the server saves the galaxy first (`/stop`).
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port         | Default | Protocol  | Required |
|--------------|---------|-----------|----------|
| Game         | 27000   | TCP + UDP | yes (primary allocation); only listened on with `--use-steam-networking false` |
| Game Query   | 27001   | UDP       | yes, primary + 1 |
| Steam Query  | 27002   | UDP       | yes, primary + 2 (Steam server list, A2S) |
| Steam Master | 27003   | UDP       | yes, primary + 3 |
| RCON         | 27015   | TCP       | only when enabled with `--rcon-password` |

Allocate the three ports following the primary allocation as additional allocations. The egg passes them to the server as `--query-port`, `--steam-query-port` and `--steam-master-port`; the server's own defaults (27003, 27020, 27021) are not used.

By default players connect through Steam networking, and the server does not listen on the Game port at all. The old direct TCP/UDP connection is only used with `--use-steam-networking false` in Extra Arguments; the server then warns that it is deprecated and unsafe, is not authenticated with Steam and does not show up in the server list.

The game client checks whether an unlisted server is online on the Game Query port **27003**. Listed servers (the default) are found through Steam and can use any ports. To run an unlisted server that players join by address, give it the primary port 27002 so its Game Query port is 27003.

## Configuration

Server Name, Max Players, Difficulty, Listed and Save Interval are passed to the server on every start and saved into the galaxy's `server.ini`. The Server Password is written into `galaxies/<Galaxy Name>/server.ini` on every start. Seed and Scenario only apply when a new galaxy is created.

Every other setting is in `galaxies/<Galaxy Name>/server.ini`; the server writes a description of all of them to `server.ini - readme.txt` next to it. Edit the file while the server is stopped.

## Admins

The Admin SteamID64 is added to the galaxy's administrators on start (`admin.xml`). More admins are added in game or from the console with `/admin -a <name or SteamID64>`. The `admin.xml` file also holds the command permissions of every group.

## Saves

Galaxies are stored in `galaxies/<Galaxy Name>/` (sectors, factions, players, `server.ini`, `admin.xml`, access lists and server logs). The server saves every Save Interval, on `/save` and on shutdown. Hourly backups are written to `.avorion/backups/`. To use an existing galaxy, stop the server, upload its folder to `galaxies/` and set Galaxy Name to the folder name.

## Requirements

About 0.2 GB of memory with a new galaxy and no players; it grows with the number of loaded sectors and players. Plan for 2-4 GB. The installation takes about 500 MB of disk; a new galaxy about 200 KB, growing as it is explored, plus its hourly backups.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the galaxies and their backups:

```
# Ignore all
*
# Except the galaxies and the automatic backups
!galaxies/
!.avorion/
```
