# Ground Branch

Ground Branch is a tactical first-person shooter from BlackFoot Studios, a spiritual successor to the early Rainbow Six and Ghost Recon games: realistic close-quarters PvP and co-op missions against AI.

Native Calagopus egg for the Ground Branch dedicated server (Steam app `476400`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, password and player limit are written to `GroundBranch/ServerConfig/Server.ini` on every start; everything else in it stays yours.
- The installer creates a co-op map rotation (Intel Retrieval on Depot, City and Creek) in `MapList.ini`.
- The server log is streamed to the console. The server reads no console input; admins use the in-game admin commands.
- The panel's stop sends SIGINT; the server exits cleanly. Ground Branch keeps no world state.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam server list, A2S) |

## Configuration

The config files are in `GroundBranch/ServerConfig/`:

| File | Contents |
|------|----------|
| `Server.ini` | Name, MOTD, passwords, player and spectator limits, game rules, scheduled shutdown |
| `MapList.ini` | Map rotation: one `MapList=` line per map, `/Game/GroundBranch/Maps/<Map>/<Map>?Mission=/Game/GroundBranch/Mission/<Map>/<Mission>?<options>`; missions include IntelRetrieval, TerroristHunt (co-op) and TeamElimination, Uplink, Deathmatch (PvP) |
| `Admin.ini`, `AdminSetupPassword.txt` | Admins (SteamID64) and the admin setup password the server generates |
| `Ban.ini`, `TeamKill.ini`, `Vote.ini` | Bans, team-kill and voting rules |

Edit them while the server is stopped. Without a map list the server stays on its empty default map.

## Admins

Admins are listed by SteamID64 in `Admin.ini` (edit it while the server is stopped). The server also generates an admin setup password in `AdminSetupPassword.txt`.

## Requirements

About 0.8 GB of memory before the first map loads and 1.3 GB on Creek with no players; plan for 4 GB. The installation takes about 2.4 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Ground Branch server configuration
!GroundBranch/ServerConfig/
```
