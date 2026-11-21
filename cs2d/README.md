# CS2D

CS2D (Counter-Strike 2D) is Unreal Software's free top-down 2D shooter inspired by Counter-Strike: terrorists against counter-terrorists in bomb, hostage, VIP, deathmatch and zombie modes, with Lua scripting and custom maps.

Calagopus egg for the CS2D Linux dedicated server. The installer downloads the current game release linked on [cs2d.com](https://www.cs2d.com/download.php) and the dedicated server binary from unrealsoftware.de.

## Capabilities

- Server name, password, RCON password and player limit are written to `sys/server.cfg` on every start; the port is a launch argument. Everything else in `sys/server.cfg`, the map cycle (`sys/mapcycle.cfg`) and the Lua scripts (`sys/lua/`) stays yours.
- Reinstalling updates the game and the server binary and keeps existing files in `sys/`. The server checks its version with U.S.G.N. on start and logs whether it is up to date.
- The server log is shown in the console. Console input is not processed (a typed `status` got no answer in testing); administer the server in game or over RCON with the RCON password.
- The server keeps no game state; the panel's stop sends SIGINT and it exits.
- Failed installations are reported to Calagopus as failed, with the reason.

The server announces itself to the U.S.G.N. server list unless `sv_lan 1` is set in `sys/server.cfg`.

## Server Ports

| Port | Default | Protocol |
|------|---------|----------|
| Game | 36963   | UDP      |

## Requirements

About 35 MB of memory with no players; plan for 256 MB. The installation takes about 75 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings, scripts and custom maps:

```
# Ignore all
*
# Except the settings, scripts and maps
!sys/
!maps/
```
