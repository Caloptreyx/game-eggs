# FOUNDRY

FOUNDRY is a first-person factory building and automation game in an infinite voxel world. Players mine resources, build production lines, research technology and expand into space, alone or together.

Calagopus egg for the FOUNDRY dedicated server (Steam app `2915550`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Name, password, ports, visibility, player limit, world, map seed, autosave interval and pause-when-empty are written to `App.cfg` on every start. The installer seeds `App.cfg` with every [documented option](https://dedicated.foundry-game.com/).
- The Unity log is written to `server_output.log` and streamed to the console. The server reads no console input.
- The panel's stop saves the world and stops the server.
- Failed installations are reported to Calagopus as failed, with the reason.

The Windows server needs a display, so it runs under Xvfb, and it needs the game's Steam app id (`983870`, as in the shipped launcher). A signal kills it without saving. The startup command therefore runs it in its own session and catches the panel's stop signal: it asks the server window to close (`wine taskkill`), and the server saves an autosave and exits.

Saves and logs are kept in `saves/` (`server_persistent_data_override_folder`): worlds in `saves/save/<World Name>/`, the Unity player log in `saves/Player.log`. A new World Name creates a new world from the map seed.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 3724    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S; used when Public) |

## Requirements

About 3.2 GB of memory with a new world and no players; plan for 6 GB. The installation, with the Proton prefix, takes about 2.7 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and settings:

```
# Ignore all
*
# Except the worlds and the server config
!saves/save/
!App.cfg
```
