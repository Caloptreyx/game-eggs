# Left 4 Dead 2

Valve's co-op zombie shooter: four survivors fight their way through the campaigns, against the AI Director's hordes or other players as the Infected in Versus.

Native Calagopus egg for the Source dedicated server (Steam app `222860`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Game mode (`mp_gamemode`), start map, player count, and extra launch parameters or convars (e.g. `+z_difficulty Hard`).
- Server name, password and RCON password are written to `left4dead2/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel c2m1_highway`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

A fresh SteamCMD install of the Linux server fails with "Invalid platform", so the installer downloads the Windows files first and then updates the installation to the Linux platform.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON) |

In our test the server (version 2.2.4.3) registered with Steam (`Connection to Steam servers successful`, `status` showing the public address) but did not answer Steam A2S server queries on the game port, from outside or inside the container. Players join with `connect <ip>:<port>` in the game console.

## Configuration

| Path | Contents |
|------|----------|
| `left4dead2/cfg/server.cfg` | Server settings (created by the installer) |
| `left4dead2/addons/` | Server plugins (Metamod:Source, SourceMod) and add-on content |

Game modes include `coop`, `realism`, `versus`, `survival` and `scavenge`; the start map has to support the mode. Co-op allows 4 players, Versus 8.

## Stopping

Stopping sends `quit`; the server shuts down within seconds. Left 4 Dead 2 keeps no world saves. The egg turns core dumps off so a crash leaves no core files behind.

## Requirements

About 0.2 GB of memory for an empty, hibernating server; more with players. The installation takes about 9.7 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and add-ons:

```
# Ignore all
*
# Except the server configuration and add-ons
!left4dead2/cfg/
!left4dead2/addons/
```
