# Tachyons CE

Tachyons Community Edition is the community-maintained version of Tachyon, a free multiplayer space game in the style of FTL: Faster Than Light. Players crew ships together, manage power, oxygen and systems, board and raid other ships and explore a persistent universe.

Calagopus egg for the Java dedicated server from the [TachyonsCE releases](https://github.com/Dav-Edward/TachyonsCE/releases). Only the `server/` folder of the release is installed; players download the full release for the client.

## Capabilities

- Installs the latest release or a chosen tag (Version); reinstall to change it. The project has had no release since v1.0.2, so there is no update on start.
- Port, autosave interval and the reset vote setting are written to `config.cfg` on every start; everything else in it stays yours. `config.cfg` and the other `.cfg` files are only created when missing, so reinstalls keep them.
- Console commands work in the panel console (`/help`, `/online`, `/save`, `/kick`, `/ban`, ...).
- The panel's stop sends `/exit`, which saves the universe and shuts the server down.
- Failed installations are reported to Calagopus as failed, with the reason.

The universe, ships, NPCs and players are kept in `DataBase/`. Delete that folder while the server is stopped to start a new universe.

## Server Ports

| Port | Default | Protocol |
|------|---------|----------|
| Game | 30303   | TCP      |

## Requirements

About 120 MB of memory with no players; plan for 1 GB. The installation takes about 4 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the universe and settings:

```
# Ignore all
*
# Except the database and the settings
!DataBase/
!*.cfg
```
