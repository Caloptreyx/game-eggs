# Frozen Flame

Frozen Flame is an open-world survival RPG. Players explore a frozen fantasy world, fight monsters and bosses, build bases and level up their characters, alone or together.

Native Calagopus egg for the Frozen Flame dedicated server (Steam app `1348640`), installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, ports and the RCON password are launch arguments; player limit and password are written to `FrozenFlame/Saved/Config/LinuxServer/Game.ini` on every start. The installer creates `Game.ini` once from the [developer's template](https://github.com/DreamsideInteractive/FrozenFlameServer), whose gameplay balance settings stay yours.
- The server log is shown in the console. The server reads no console input; use RCON for [admin commands](https://github.com/DreamsideInteractive/FrozenFlameServer/blob/main/RCON.commands), with the password in the form `admin:<RCON Password>`.
- The panel's stop saves the world and the players, then stops the server.
- Failed installations are reported to Calagopus as failed, with the reason.

The server only saves periodically, and a signal stops it without saving. The startup command therefore runs it in its own session and catches the panel's stop signal: it sends `Admin_SaveAll` over RCON (with bash's `/dev/tcp`), waits for the save, then stops the server. When RCON Password is empty, a random password is used for that session, so the stop always works.

Saves are in `FrozenFlame/Saved/SaveGames/`.

As of this egg's tests, the official server list host `meta.frozen-flame.xyz` did not resolve, so the server logs failing heartbeats and may not appear in the in-game list; players can still connect by address.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 25575   | UDP      | yes, additional allocation set as Query Port |
| RCON  | 27015   | TCP      | yes, additional allocation set as RCON Port (also used by the stop) |

## Requirements

About 3.5 to 5.6 GB of memory with no players; plan for 8 GB. The installation takes about 2.9 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the saves and settings:

```
# Ignore all
*
# Except the saves and the server config
!FrozenFlame/Saved/SaveGames/
!FrozenFlame/Saved/Config/LinuxServer/Game.ini
```
