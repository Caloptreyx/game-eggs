# BATTALION: Legacy

BATTALION: Legacy is a free WW2 first-person shooter in the spirit of the classic Call of Duty games: fast infantry combat on competitive maps in modes such as Wartide, bomb and domination.

Native Calagopus egg for the BATTALION: Legacy dedicated server (Steam app `805140`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, playlist (Queue), admin password and ports are passed on every start; further launch parameters through Extra Arguments.
- The server log is shown in the console. The server reads no console input; the panel's stop sends SIGINT.
- Failed installations are reported to Calagopus as failed, with the reason.

The game's crash reporter needs OpenSSL 1.1, which Debian 13 no longer ships: the installer adds Debian 11's `libssl1.1` libraries in `.libssl11/`. Without them the server hangs at startup. Matches keep no world state.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27016   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |

## Configuration

The server's settings are in `Battalion/Saved/Config/LinuxServer/` (created on the first start), with the game settings in `BattalionSettings.ini` and `Game.ini`. The playlist decides the maps and modes (`Wartide` by default; the server rotates through its maps).

## Requirements

About 390 MB of memory with no players; plan for 2 GB. The installation takes about 2.2 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the server configuration
!Battalion/Saved/Config/
```
