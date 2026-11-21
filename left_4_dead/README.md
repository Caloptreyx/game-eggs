# Left 4 Dead

Valve's original co-op zombie shooter: four survivors fight their way through the campaigns (No Mercy, Death Toll, Dead Air, Blood Harvest, The Sacrifice, Crash Course), against the AI Director's hordes or other players as the Infected in Versus.

Native Calagopus egg for the Source dedicated server (Steam app `222840`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Game mode (`mp_gamemode`), start map, player count, and extra launch parameters or convars (e.g. `+z_difficulty Hard`).
- Server name, password and RCON password are written to `left4dead/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel l4d_airport01_greenhouse`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

A fresh SteamCMD install of the Linux server can fail with "Invalid platform", so the installer downloads the Windows files first and then updates the installation to the Linux platform.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON) |


## Configuration

| Path | Contents |
|------|----------|
| `left4dead/cfg/server.cfg` | Server settings (created by the installer) |
| `left4dead/addons/` | Server plugins (Metamod:Source, SourceMod) and add-on content |

Game modes are `coop`, `versus` and `survival`; the start map has to support the mode. Co-op allows 4 players, Versus 8.

## Stopping

Stopping sends `quit`; the server shuts down within seconds. Left 4 Dead keeps no world saves. The egg turns core dumps off so a crash leaves no core files behind.

## Requirements

About 0.1 GB of memory for an empty, hibernating server; more with players. The installation takes about 8.6 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and add-ons:

```
# Ignore all
*
# Except the server configuration and add-ons
!left4dead/cfg/
!left4dead/addons/
```
