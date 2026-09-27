# Project Zomboid

Project Zomboid is an open world isometric zombie survival game: loot, build, craft, fight and survive for as long as you can in a zombie infested Kentucky.

Native Calagopus egg for the Project Zomboid dedicated server (Steam app `380870`), installed anonymously through SteamCMD. It uses the **native Linux build** with its bundled Java runtime and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Stable release by default (Build 42); beta branches such as `unstable` (the next Build 42 release) or `legacy41` (Build 41) through the Beta Branch variable.
- Name, description, visibility, passwords, max players, ports, RCON and mods are written to the server config on every start.
- The console passes commands straight to the server, e.g. `help`, `players`, `save`, `servermsg "Restart in 5 minutes"`, `kick <user>`, `setaccesslevel <user> admin`.
- Stopping the server sends `quit`, which saves the world before the server exits.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 16261   | UDP      | yes (primary allocation) |
| UDP  | 16262   | UDP      | yes |
| RCON | 27015   | TCP      | only when an RCON Password is set |

UPnP is turned off: the ports are opened through the allocations.

## Configuration

The server keeps its data in the `Zomboid/` folder of the server root. The server is always started as `servertest`, so its config files are:

- `Zomboid/Server/servertest.ini`: server settings (PvP, safehouses, chat, backups, ...).
- `Zomboid/Server/servertest_SandboxVars.lua`: world settings (zombie count and behaviour, loot, time, ...).
- `Zomboid/Server/servertest_spawnregions.lua` / `servertest_spawnpoints.lua`: spawn locations.

The server writes them with every setting and its description on the first start. The panel-managed keys (`PublicName`, `PublicDescription`, `Public`, `Password`, `MaxPlayers`, `DefaultPort`, `UDPPort`, `UPnP`, `RCONPort`, `RCONPassword`, `Mods`, `WorkshopItems`) are set from the variables on every start; edit everything else in the files while the server is stopped.

The Admin Password is the password of the in-game account `admin`, set on every start. Join with the user name `admin` to use the admin commands in the chat.

### Memory

Java Memory sets the maximum Java heap (`-Xmx`). The process needs 1-2 GB on top of that (Java runtime, native libraries), so set the server's memory limit at least 2 GB above the Java Memory value, or the server is killed when the heap fills up.

### Mods

Put the Workshop item IDs in Workshop Items and the mod IDs (from each mod's `mod.info`) in Mods, both separated by semicolons. Map mods also need their folder name added to `Map=` in `servertest.ini`, before `Muldraugh, KY`. The server downloads Workshop items on start.

## Saves

The world is saved in `Zomboid/Saves/Multiplayer/servertest/`, the player accounts and bans in `Zomboid/db/servertest.db`. The server saves on `save` and `quit`, and every `SaveWorldEveryMinutes` minutes when set in `servertest.ini`. Automatic backups (`BackupsPeriod`, `BackupsCount`) go to `Zomboid/backups/`.

Worlds made on Build 41 (`legacy41`) do not load on Build 42. Switching between them needs a new world (delete or move `Zomboid/Saves/Multiplayer/servertest/`) and a reinstall.

## Requirements

Java Memory plus 2 GB. With the default Java Memory (4 GB), an empty world uses about 5 GB in total; plan for 8 GB or more with players and mods. The installation takes about 7 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your config and saves:

```
# Ignore all
*
# Except the Project Zomboid config and saves
!Zomboid/
Zomboid/backups/
```
