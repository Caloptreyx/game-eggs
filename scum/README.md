# SCUM

Open world survival game with in-depth character simulation, crafting, base building and PvP on a prison island.

Native Calagopus egg. The dedicated server (Steam app `3792580`) only exists for Windows. It is installed anonymously through SteamCMD and runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Name, password, max players and playstyle are written to `ServerSettings.ini` by the panel, so names may contain quotes, `$` and brackets.
- The server log is streamed to the console. The server reads no console input and has no RCON; admin commands are entered in the in-game chat by players listed in `AdminUsers.ini`.
- Stopping the server closes it the way Windows does: it saves the world and closes the database before exiting.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, for the server browser (Query Port variable, own allocation) |

Players join with the game port. The Query Port cannot be between 27020 and 27050 when Steam runs on the same host.

The server also opens TCP on the game port + 2 (7779 by default), which the game's guides list as optional RCON. It is not an official RCON, and the panel does not use it.

## Configuration

| File | Contents |
|------|----------|
| `SCUM/Saved/Config/WindowsServer/ServerSettings.ini` | Server settings (`[General]`, `[World]`, `[Respawn]`, ...) |
| `SCUM/Saved/Config/WindowsServer/AdminUsers.ini` | Admins, one SteamID64 per line |
| `SCUM/Saved/Config/WindowsServer/ServerSettingsAdminUsers.ini` | Players allowed to change server settings in game |
| `SCUM/Saved/Config/WindowsServer/BannedUsers.ini`, `WhitelistedUsers.ini`, `ExclusiveUsers.ini`, `SilencedUsers.ini` | Player lists |
| `SCUM/Saved/Config/WindowsServer/EconomyOverride.json`, `RaidTimes.json`, `Notifications.json` | Economy, raid times, server messages |

The server creates these files on its first start and fills in missing settings on every start. Server name, password, max players and playstyle are panel variables, written to `ServerSettings.ini` on every start, so change those in the panel. Edit the rest only while the server is stopped.

## Saves

The world and players are stored in the SQLite database `SCUM/Saved/SaveFiles/SCUM.db`. The server copies it to `SCUM_<date>.db-backup` next to it on every start and never deletes these copies.

## Proton notes

- The server hangs during startup without a display, so the egg starts a virtual one (Xvfb).
- The Proton prefix is set to report Windows 7. On newer Windows versions the server checks the signatures of system DLLs, which fails under Proton (`DLL signature verification failed, disabling multiplayer`) and disables multiplayer.
- Ctrl+C (SIGINT) kills the server without saving. The egg therefore closes the server window instead, which makes it save and exit.

## Requirements

About 10 GB of memory right after startup with an empty world, growing with players, vehicles and bases; plan for 12-16 GB. The server fails to start below about 10 GB. The installation takes about 17 GB of disk.

Startup takes about a minute; the server is reported as running once it logs its first `Global Stats` line.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your config and saves:

```
# Ignore all
*
# Except the SCUM saves and config
!SCUM/Saved/
SCUM/Saved/Logs/
SCUM/Saved/SaveFiles/Logs/
```
