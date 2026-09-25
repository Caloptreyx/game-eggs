# Conan Exiles

Survive in the brutal lands of Conan the Barbarian.

Native Calagopus egg for the Conan Exiles dedicated server (Steam app `443030`), installed anonymously through SteamCMD. It uses the **native Linux build** that the server now ships, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- The server log is streamed to the console, and console commands are sent over RCON once the server is up, e.g. `listplayers`, `broadcast <message>`.
- Name, passwords, max players, PvP, BattlEye, region and RCON are written to the config files on every start.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game  | 7777  | UDP | yes (primary allocation); the game also uses the next port (7778, UDP) |
| Query | 27015 | UDP | yes, for the server browser |
| RCON  | 25575 | TCP | only for remote admin tools; the panel console uses it locally |

Add the port after the game port (7778 by default) as an allocation too.

## Configuration

The config files are in `ConanSandbox/Saved/Config/LinuxServer/`:

| File | Contents |
|------|----------|
| `ServerSettings.ini` | Gameplay settings: rates, PvP schedule, building, thralls, purges, ... |
| `Engine.ini` | Server name |
| `Game.ini` | Max players, RCON |

The server rewrites these files while it runs, so edit them only while it is stopped. The panel-managed settings are rewritten on every start, so change those in the panel.

## Admins

Set an Admin Password, join the server, open Settings > Server Settings, enter the password and click Make Me Admin.

## Saves

The world is saved in `ConanSandbox/Saved/game_0.db`, with automatic backups (`game_0_backup_*.db`) next to it.

## Requirements

About 6-8 GB of memory and 5 GB of disk, more with mods.
