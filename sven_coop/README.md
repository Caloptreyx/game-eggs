# Sven Co-op

Sven Co-op is a cooperative first-person shooter on the GoldSource (Half-Life) engine: players team up against AI-controlled enemies in custom campaigns, including the full Half-Life campaign.

Native Calagopus egg for the Sven Co-op dedicated server (Steam app `276060`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Any map, player count, and extra launch parameters or cvars.
- Server name, password and RCON password are written to `svencoop/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel hl_c01_a1`, `kick <name>`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and Steam server queries) |

## Files

| Path | Contents |
|------|----------|
| `svencoop/server.cfg` | Server settings |
| `svencoop/mapcycle.txt` | Map rotation |
| `svencoop/banned.cfg`, `svencoop/listip.cfg` | Banned players and IP addresses |
| `svencoop_addon/` | Custom maps and content |

## Stopping

Stopping sends `quit`; the server shuts down within seconds. Sven Co-op keeps no world saves. The egg turns core dumps off so a crash leaves no core files behind.

## Requirements

About 0.1 GB of memory for an empty server; more with players and large maps. Plan for 512 MB-1 GB. The installation takes about 2.9 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and add-on content:

```
# Ignore all
*
# Except the server configuration and add-on content
!svencoop/server.cfg
!svencoop/mapcycle.txt
!svencoop/banned.cfg
!svencoop/listip.cfg
!svencoop_addon/
```
