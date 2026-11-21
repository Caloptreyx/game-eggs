# Space Station 14

Space Station 14 is a free multiplayer space station roleplay simulator, the remake of Space Station 13: every round, the crew of a station tries to keep it running while antagonists, accidents and disasters get in the way.

Native Calagopus egg for the Space Station 14 server (`Robust.Server`), installed from the build manifest of the official Space Wizards CDN (or any fork's manifest). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image with the .NET runtime and ICU libraries the installer adds; no Steam account is involved.

## Capabilities

- Installs the newest build of the chosen fork (Build Manifest; the default is the official Wizard's Den build). Reinstall to update; `server_config.toml` and `data/` are kept.
- Server name, description, player limit, hub listing and connect address are passed as `--cvar` options on every start; they take precedence over `server_config.toml`.
- Console commands go straight to the server, e.g. `restartround`, `setgamepreset <preset>`, `kick`, `ban`. Stopping the server sends `shutdown`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol  | Required |
|------|---------|-----------|----------|
| Game | 1212    | UDP + TCP | yes (primary allocation): UDP for the game, TCP for the status API the launcher and hub use |

`http://<address>:<port>/status` shows the server's name, player count and round.

## Configuration

`server_config.toml` (the example config shipped with the build, kept on reinstall) holds every other setting: game presets, lobby duration, role timers, database, admin settings, ... Add Extra Arguments such as `--cvar game.lobbyduration=60` for single overrides.

Advertising the server in the public hub (Advertise) means following the hub rules; leave it off for private servers, which players join by address (`ss14://<address>:<port>`).

## Admins

Admins are kept in the server database (`data/preferences.db`). Give the first host admin rights with the `promotehost <username>` console command, then manage admins in game with the admin menu.

## Saves

Rounds are not saved: every round starts a fresh station. Player preferences, characters, admin data and bans are kept in the SQLite database `data/preferences.db`.

## Requirements

About 0.7 GB of memory with no players; plan for 2-4 GB, more for busy rounds. The installation (server build, .NET runtime and ICU) takes about 275 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings and database:

```
# Ignore all
*
# Except the Space Station 14 settings and database
!server_config.toml
!data/
```
