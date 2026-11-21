# Archean

Archean is an engineering sandbox set on a primordial Earth and its Moon. Players design and build vehicles, bases, rockets and space stations from modular blocks, wire and program them, alone or together.

Native Calagopus egg for the Archean dedicated server (Steam app `3557220`), installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, public listing, password, player limit, port, world, spawn body, autosave interval and tick settings are written to `Archean-data/server/server.ini` on every start. The installer seeds the file with every documented setting; because `server_public_name` is set, the server skips its interactive first-run questions. Other settings, such as `external_mods_dir`, stay yours.
- The server log is shown in the console.
- The panel's stop sends SIGINT; the server saves the world and exits.
- Failed installations are reported to Calagopus as failed, with the reason.

Worlds are saved in `Archean-data/server/worlds/<World Name>/`; a new World Name creates a new world. The game mode of a world (Creative or Adventure) cannot be set in `server.ini`. Admins are listed by Steam64 ID in `Archean-data/server/admins.txt`, banned players in `blacklist.txt`. See the [server documentation](https://wiki.archean.space/en/multiplayer/settingup.html) for Workshop mods.

## Server Ports

| Port | Default | Protocol |
|------|---------|----------|
| Game | 8881    | UDP and TCP |

## Requirements

About 1.9 GB of memory with a new world and no players; plan for 8 GB (the developer's minimum). The installation takes about 3.6 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and settings:

```
# Ignore all
*
# Except the worlds and settings
!Archean-data/
```
