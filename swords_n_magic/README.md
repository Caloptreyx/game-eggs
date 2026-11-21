# Swords 'n Magic and Stuff

Swords 'n Magic and Stuff is a cozy co-op open-world RPG. Up to 16 players explore, fight, quest, cook and dress up together on a shared server whose world resets daily.

Calagopus egg for the Swords 'n Magic and Stuff dedicated server (Steam app `2058450`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton (with Xvfb) in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Name, owner, password, player limit and the reset schedule are passed as the server's `SNM_*` environment variables, which override `SNM2020/Saved/Config/WindowsServer/Game.ini`; the ports are launch arguments.
- The Unreal log is streamed to the console; the server's own summary log is written to `SNM2020/Saved/Logs/SnMDedSrv-<timestamp>.log`. The server reads no console input.
- The panel's stop sends SIGINT, which the shell forwards to the server.
- Failed installations are reported to Calagopus as failed, with the reason.

Owner Steam ID is required: the server stops at once with "Invalid DedSrv_OwnerSteamID" without a valid SteamID64. The owner gets the admin panel and can add moderators in game. The server name must be 2 to 5 different words from the game's approved list, which the server writes to `SNM2020/Saved/Logs/Allowed-Words.txt` on every start; leave it empty for a random name. By design, the world resets on the schedule set by Days Until Reset and Reset Time Of Day, and the developer strongly recommends daily resets.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam session browser, A2S) |

## Requirements

About 2.1 GB of memory with no players; plan for 4 GB. The installation, with the Proton prefix, takes about 2 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and saves:

```
# Ignore all
*
# Except the config and saves
!SNM2020/Saved/Config/
!SNM2020/Saved/SaveGames/
```
