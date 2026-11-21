# Squad

Squad is a large-scale tactical combined-arms shooter: up to 100 players in two teams fight over vast maps with vehicles, logistics, base building and close communication between squads.

Native Calagopus egg for the Squad dedicated server (Steam app `403240`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- Server name, max players, reserved slots, server browser visibility, map rotation randomization and RCON are set from the panel on every start.
- Console input: lines typed in the panel console are sent to the server over RCON, e.g. `ShowServerInfo`, `ListPlayers`, `ShowCurrentMap`, `AdminBroadcast <message>`, `AdminKick <name or id> <reason>`, `AdminChangeLayer <layer>`, `AdminEndMatch`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port   | Default | Protocol | Required |
|--------|---------|----------|----------|
| Game   | 7787    | UDP      | yes (primary allocation) |
| Query  | 27165   | UDP      | yes, additional allocation set as Query Port |
| Beacon | 15000   | UDP      | yes, additional allocation set as Beacon Port |
| RCON   | 21114   | TCP      | additional allocation set as RCON Port; reachable from outside only for remote RCON tools |

The server registers its session with Offworld's online services (Epic Online Services), which is how players find it in the server browser; in our test it did not answer Steam A2S queries on the Query Port.

## Configuration

The config files are in `SquadGame/ServerConfig/`. On every start the panel writes Server Name, Max Players, Reserved Slots and Advertise into `Server.cfg`, and the RCON Port and Password into `Rcon.cfg`; Max Players and Random Rotation are also passed on the command line (`FIXEDMAXPLAYERS`, `RANDOM`). Everything else is edited in those files while the server is stopped:

- `Server.cfg`: other server settings (team balance, voting, tickets, ...).
- `LayerRotation.cfg` / `MapRotation.cfg`: the rotation; `ExcludedLayers.cfg`, `ExcludedFactions.cfg`, ...
- `Admins.cfg`: admin groups and members (SteamID64), including the `reserve` permission for reserved slots.
- `MOTD.cfg`, `ServerMessages.cfg`, `Bans.cfg`, `License.cfg` (licensed servers).

The RCON Password is required (at least 6 characters, no spaces): the console uses it.

## Saves

Squad keeps no world state: every match starts fresh. Only `SquadGame/ServerConfig/` is worth backing up. The panel's stop sends SIGINT, which shuts the server down cleanly within seconds.

## Requirements

About 4-4.4 GB of memory on a large map (Al Basrah) with no players; plan for 8 GB, more with a full server. The installation takes about 14 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Squad server configuration
!SquadGame/ServerConfig/
```
