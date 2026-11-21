# America's Army: Proving Grounds

America's Army: Proving Grounds is the U.S. Army's free tactical team shooter on Unreal Engine 3, with small-unit combat, realistic weapons and roles.

Native Calagopus egg for the America's Army: Proving Grounds dedicated server (Steam app `203300`), installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Ports and the server name are launch arguments. The game and admin passwords and the server name are also written to `AAGame/Config/DefaultGame.ini` on every start, and the engine rebuilds its generated `AAGame/Config/PCServerLinux-AAGame.ini` from it.
- The server log is shown in the console and written to `AAGame/Logs/Server.log`.
- The panel's stop sends SIGINT, and the server shuts down. Matches keep no state.
- Failed installations are reported to Calagopus as failed, with the reason.

The server binary links `libGL.so.1` even though it renders nothing; the installer puts Debian's libGL libraries into `.gl/`. The server rotates through its built-in maps and reports 24 slots, set by the game type. PunkBuster is enabled by default.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7778    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |
| Auth  | 8778    | UDP      | yes, additional allocation set as Auth Port (Steam authentication) |

## Requirements

About 250 MB of memory with no players; plan for 1 GB. The installation takes about 2.5 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the game configuration
!AAGame/Config/
```
