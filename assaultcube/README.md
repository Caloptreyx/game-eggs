# AssaultCube

AssaultCube is a free and open source multiplayer first-person shooter based on the Cube engine: fast, lightweight matches in team deathmatch, capture the flag, one shot one kill and more, with realistic-looking weapons and maps.

Native Calagopus egg for the official AssaultCube Linux server (`bin_unix/linux_64_server`, from the [AssaultCube GitHub releases](https://github.com/assaultcube/AC/releases)). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Install of the latest release or a fixed tag (AssaultCube Version); reinstall to update. `config/`, added maps and the server key are kept.
- The installer generates a private server key in `config/servercmdline.txt` (the shipped example uses a public placeholder).
- Server name, message of the day, player limit, passwords and master server are set from the panel on every start.
- The server registers with the AssaultCube master server (`ms.cubers.net`); set Master Server to `localhost` to keep it private.
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads no console input; admins log in with `/setadmin 1 <password>` in game. The panel's stop sends SIGINT and the server exits at once. AssaultCube keeps no world state; server logs are written to `logs/`.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 28763   | UDP      | yes (primary allocation) |
| Info | 28764   | UDP      | yes: always the game port + 1 (server browser pings); allocate it too |

## Configuration

| File | Contents |
|------|----------|
| `config/servercmdline.txt` | Further command-line switches, one per line (vote permissions, kick/ban thresholds, demo settings, ...); see `docs/commandline.html` |
| `config/maprot.cfg` | Map rotation rules (modes, weights, game time per map) |
| `config/serverpwd.cfg` | Additional admin passwords |
| `config/serverblacklist.cfg`, `config/nicknameblacklist.cfg` | IP and nickname blacklists |

Custom maps go in `packages/maps/servermaps/`.

## Requirements

About 6 MB of memory with no players; plan for 128 MB. The installation takes about 65 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and custom maps:

```
# Ignore all
*
# Except the configuration and custom maps
!config/
!packages/maps/servermaps/
```
