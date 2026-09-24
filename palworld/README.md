# Palworld

Fight, farm, build and work alongside mysterious creatures called "Pals" in this completely new multiplayer, open world survival and crafting game!

Native Calagopus egg. The dedicated server (Steam app `2394010`) is installed anonymously through SteamCMD and runs in the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- The main settings (name, passwords, max players, rates, death penalty, PvP, ...) are set from the panel on every start. Everything else stays editable in `PalWorldSettings.ini`.
- The console sends commands to the server over RCON, e.g. `Info`, `ShowPlayers`, `Save`, `Broadcast Hello`, `KickPlayer <SteamID>`.
- Stopping the server saves the world first. `Shutdown` alone does not save in Palworld, so the console always sends `Save` before any `Shutdown` command.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 8211    | UDP      | yes (primary allocation) |
| RCON  | 25575   | TCP      | only for remote admin tools; the panel console uses it locally |

## Configuration

The config file is `Pal/Saved/Config/LinuxServer/PalWorldSettings.ini`. It is created from the server's defaults on install, and again if it is ever emptied.

The settings available as panel variables are written into it on every start, so change those in the panel. Other settings can be edited in the file, but only while the server is stopped: the server rewrites the file when it stops.

The Admin Password is also the RCON password, so it is required.

## Requirements

Palworld uses a lot of memory, and usage grows with players, bases and uptime. Plan for at least 8 GB, better 16 GB or more, and schedule regular restarts. The installation takes about 5 GB of disk.

## Saves

The world is saved in `Pal/Saved/SaveGames/0/<world id>/`. With `bIsUseBackupSaveData=True` (the default) the server keeps backups under `backup/world/`.

To move a co-op world to the server, player IDs have to be converted, e.g. with the [Palworld Save Converter](https://physgun.com/tools/palworld-save-converter/).

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your config and saves:

```
# Ignore all
*
# Except the Palworld saves and config
!Pal/Saved/
```
