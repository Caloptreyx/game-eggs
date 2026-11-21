# Longvinter

Longvinter is a cozy-looking open-world sandbox with crafting, fishing, farming, building, trading and PvP on a shared island.

Native Calagopus egg for the Longvinter dedicated server (Steam app `1639880`), installed anonymously through SteamCMD. The developer moved the server from GitHub to Steam; the server is listed through Epic Online Services (EOS).

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Name, MOTD, website, tag, password, player limit, admin EOS IDs, PvP, tent decay, max tents, hardcore mode and the daily restart hour are written to `Longvinter/Saved/Config/LinuxServer/Game.ini` on every start. The file is created once from the shipped `Game.ini.default`; other settings (co-op play, VPN check, chest respawn, wandering traders, backups, hardcore multipliers) stay yours.
- The Unreal log is shown in the console. The server reads no console input.
- The panel's stop sends SIGINT; the server shuts down EOS and exits cleanly.
- Failed installations are reported to Calagopus as failed, with the reason.

The island is kept in `Longvinter/Saved/SaveGames/`; the server writes its save files as the world changes. Admins are identified by EOS ID (shown in the game settings), not by Steam ID. See the [server configuration guide](https://wiki.longvinter.com/server/configuration) for all `Game.ini` options.

## Server Ports

| Port | Default | Protocol |
|------|---------|----------|
| Game | 7777    | UDP      |

## Requirements

About 3.1 GB of memory with no players; plan for 6 GB. The installation takes about 2.1 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the island and settings:

```
# Ignore all
*
# Except the saves and the server config
!Longvinter/Saved/SaveGames/
!Longvinter/Saved/Config/LinuxServer/Game.ini
```
