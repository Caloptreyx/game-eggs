# Operation: Harsh Doorstop

Operation: Harsh Doorstop is a free large-scale tactical military shooter on Unreal Engine 4: combined-arms battles with infantry and vehicles, in the spirit of Squad and Project Reality, with Workshop mod support.

Native Calagopus egg for the Operation: Harsh Doorstop dedicated server (Steam app `950900`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, start map, player limit, ports and RCON are passed on every start; further launch parameters through Extra Arguments.
- The server log is shown in the console. Console lines are sent to the server over RCON (local only), so the RCON password is required; the server answers with its RCON responses (commands follow the game's RCON command set).
- The panel's stop sends SIGINT and the server shuts down. Matches keep no world state.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27005   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |
| RCON  | 7779    | TCP      | the panel console uses it locally; an allocation only for remote RCON tools |

## Configuration

The map is given as `<map>?<options>` (Map variable, default `AAS-TestMap`), followed by `?MaxPlayers=`. The game's own settings are in `HarshDoorstop/Saved/Config/LinuxServer/` (created on the first start); see the game's server documentation for map rotations, game modes and admin lists.

## Requirements

About 390 MB of memory with no players on the test map; plan for 4 GB for full servers on large maps. The installation takes about 7 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the server configuration
!HarshDoorstop/Saved/Config/
```
