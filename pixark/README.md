# PixARK

PixARK is a voxel survival sandbox in the ARK universe. Players tame dinosaurs, gather, craft and build in a procedurally generated cube world, alone or together.

Calagopus egg for the PixARK dedicated server (Steam app `824360`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton (with Xvfb) in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, passwords, player limit, world name, seeds and all ports are map URL options and launch arguments built from the panel variables on every start.
- The server log is streamed from `ShooterGame/Saved/Logs/ShooterGame.log` to the console. The server reads no console input; use RCON (with the admin password) for admin commands such as `saveworld`, `listplayers` or `broadcast`.
- The panel's stop saves the world and stops the server.
- Failed installations are reported to Calagopus as failed, with the reason.

The server needs the game's Steam app id (`593600`) to register with Steam; the startup command writes `steam_appid.txt` and sets `SteamAppId`. A signal kills it without saving. The startup command therefore runs it in its own session and catches the panel's stop signal: it sends `saveworld` and `DoExit` over RCON with bash's `/dev/tcp`. When Admin Password is empty, a random password is used for that session, so the stop always works.

Worlds are saved in `ShooterGame/Saved/CubeServers/CubeWorld_Light/<World Name>/`; a new World Name creates a new world from the seeds. Server settings are kept in `ShooterGame/Saved/Saves/` (`-ConfigsUseAltDir`).

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |
| Cube  | 15000   | UDP/TCP  | yes, additional allocation set as Cube Port |
| RCON  | 27020   | TCP      | yes, additional allocation set as RCON Port (also used by the stop) |

## Requirements

About 3.7 GB of memory with a new world and no players; plan for 8 GB. The installation, with the Proton prefix and a new world, takes about 4.6 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and settings:

```
# Ignore all
*
# Except the worlds and settings
!ShooterGame/Saved/CubeServers/
!ShooterGame/Saved/Saves/
```
