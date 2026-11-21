# HumanitZ

HumanitZ is an open world zombie survival game: scavenge, craft, build a base and survive the hordes, alone or with friends.

Native Calagopus egg for the HumanitZ dedicated server (Steam app `2728330`), installed anonymously through SteamCMD. It uses the **native Linux build** from the `linuxbranch` branch, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, passwords, max players, save name, server list pool (Search ID), PvP, save interval and RCON are written to `HumanitZServer/GameServerSettings.ini` on every start; other settings in that file are kept.
- Console input: lines typed in the panel console are sent to the server over RCON, e.g. `info`, `Players`, `save`, `admin <message>`, `kick <NetID>`, `ban <NetID>`, `season <season>`, `weather <weather>`.
- Stopping the server saves the world first, then shuts it down.
- Failed installations are reported to Calagopus as failed, with the reason.

Use the panel's stop and restart buttons, not the RCON commands `shutdown`, `RestartNow` or `QuickRestart`: the panel treats a server that exits on its own as crashed and starts it again.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port |
| RCON  | 8888    | TCP      | additional allocation set as RCON Port; reachable from outside only for remote RCON tools |

The in-game server list works through Epic Online Services (the Search ID pool); in our test the server did not answer Steam A2S queries on its Query Port.

## Configuration

`HumanitZServer/GameServerSettings.ini` is created from the shipped `REF_GameServerSettings.ini` on installation. The panel variables are written into it on every start: Server Name, Server Password, Admin Password, Max Players, Save Name, Search ID, PvP, Save Interval, and RCON (always on, with the RCON Port and Password, since the console uses it). Everything else (XP multiplier, death penalty, loot, zombie difficulty, air drops, ...) is documented in comments in that file; edit it while the server is stopped.

The RCON Password is required (at least 3 characters, no spaces or double quotes). Server Name, passwords and Search ID cannot contain double quotes. Avoid the word "Official" in the Server Name, or the server may not start or not be listed.

## Admins

Players get admin rights in game with `/AdminAccess <Admin Password>` in the chat. Permanent admins are listed one NetID per line in `HumanitZServer/AdminList.txt` (create it if missing); `Players` in the console shows the NetIDs of connected players.

## Saves

Worlds are stored in `HumanitZServer/Saved/SaveGames/SaveList/Default/<Save Name>/`. The server saves every Save Interval (default 300 seconds), on `save` in the console and when the server is stopped from the panel. A new Save Name starts a new world; the old one is kept.

## Requirements

About 4-4.7 GB of memory with a new world and no players; plan for 8 GB, more with many players. The installation takes about 1.8 GB of disk; a world about 12 MB.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and settings:

```
# Ignore all
*
# Except the HumanitZ worlds and settings
!HumanitZServer/Saved/SaveGames/
!HumanitZServer/GameServerSettings.ini
!HumanitZServer/*.txt
```
