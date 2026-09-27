# Icarus

PvE survival: drop from an orbital station onto the hostile planet Icarus, gather, craft and build to survive a prospect, then return to orbit before time runs out.

Native Calagopus egg. The dedicated server (Steam app `2089300`) only exists for Windows. It is installed anonymously through SteamCMD and runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- The server log is streamed to the console. The server reads no console input and has no RCON; admin commands are typed into the in-game chat after `/AdminLogin <Admin Password>`.
- Password, admin password, max players and prospect settings are written to `ServerSettings.ini` on every start. The Server Name is passed on the command line and cannot contain `"` or `\`; the passwords cannot either, the Admin Password also no spaces.
- The server stays up while nobody is connected: the idle shutdown timers are disabled on every start.
- Stopping the server shuts it down cleanly (see Stopping).
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 17777   | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, for the server browser (Query Port variable) |

## Configuration

`Icarus/Saved/Config/WindowsServer/ServerSettings.ini` holds the server settings (`[/Script/Icarus.DedicatedServerSettings]`). The panel variables are written to it on every start, so change those in the panel. `ShutdownIfNotJoinedFor` and `ShutdownIfEmptyFor` are always set to `-1` (never): with the game defaults (300 seconds) an idle server shuts its prospect down and returns to the lobby.

The server name is passed with `-SteamServerName`; the `SessionName` entry in the file has no effect.

## Prospects

Without a prospect to load, the server starts in a lobby and waits there. Players then create or load a prospect in the game (admin-only if Non-Admins Launch Prospects is `False`). Resume Prospect (default) resumes the last prospect on the next start; Load Prospect loads a specific save. A new prospect can also be created on start with Extra Arguments, e.g. `-CreateProspect="OpenWorld_Styx 2 false MyWorld"` (type, difficulty 1-4, hardcore, save name); remove it again afterwards. Unknown prospect types are rejected in the log (`CreateNewProspect - Failed to find prospect`).

## Stopping

Stopping sends `SIGINT`. Wine runs the server in its own session, so the startup command forwards the signal to it; the server then shuts down (`*** INTERRUPTED *** : SHUTTING DOWN` ... `LogExit: Exiting.`) within a few seconds and the container exits afterwards.

## Proton notes

- The server hangs early in startup (after `MLSDK not found`) without a display, so the startup command runs it on a virtual Xvfb display.
- `SteamAppId=1149460` (the game's app id) is set for the server; without it Steam fails to initialize (`[AppId: 0] Game Server API initialized 0`).

## Saves

Prospects are saved in `Icarus/Saved/PlayerData/DedicatedServer/Prospects/`, player data next to it in `Icarus/Saved/PlayerData/`.

## Requirements

About 1.5 GB of memory in the lobby. A running prospect with players needs more, depending on the map and the number of players. The installation takes about 10 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your config and saves:

```
# Ignore all
*
# Except the Icarus saves and config
!Icarus/Saved/
```
