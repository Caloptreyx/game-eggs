# Mindustry

An automation tower defense RTS: build production chains to supply your turrets, defend against waves of enemies or fight other players.

Native Calagopus egg for the official Mindustry dedicated server (`server-release.jar` from the [GitHub releases](https://github.com/Anuken/Mindustry/releases)). It runs on the `ghcr.io/caloptreyx/java` images (Eclipse Temurin JRE).

## Capabilities

- Any release: `latest`, or a release tag such as `v146`. The download is checked against GitHub's published checksum where one exists.
- Name, description, max players and autosave settings are applied from the panel on every start.
- The server resumes from its save slot, or hosts the selected map and game mode when there is none yet.
- Console commands go straight to the server, e.g. `help`, `status`, `players`, `maps all`, `say <message>`, `save <slot>`, `kick <name>`.
- Stopping the server sends `exit`. The console saves the game to the save slot first, since `exit` alone does not save.
- Failed installations (for example a version that does not exist) are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 6567    | TCP and UDP | yes (primary allocation) |

The panel passes the allocation's port to the server on every start.

## Configuration

The panel variables are sent to the server as console commands (`config name ...`, `playerlimit ...`, `config autosave ...`) on every start, so change them in the panel. Name and description cannot contain commas: the server splits its startup commands at commas.

Every other setting (`config` without arguments lists them) can be changed in the console with `config <name> <value>`. The server stores them in `config/settings.bin`, so they persist across restarts. Global rules are set with `rules add <name> <value>` or in `config/rules.hjson`.

## Maps and game modes

Map and Game Mode are used only when there is no save yet. Built-in maps are listed with `maps all`; custom `.msav` maps go into `config/maps/` (use `_` for spaces in the name). Pick a map that suits the mode: survival needs enemy spawns, attack and pvp need more than one team. If the map does not exist, the console shows `No map with name '...' found.` and the server does not host; fix the variable and restart.

To switch maps, delete `config/saves/<slot>.msav` and restart, or run `stop` and `host <map> <mode>` in the console. After a game over the server loads the next map by itself (see `shuffle` and `nextmap`).

## Saves

Saves are stored in `config/saves/`. The save slot (`server` by default) is written on stop and loaded on the next start. With Autosave on, the server also writes `auto_<map>_<date>.msav` at the set interval and keeps the newest ones; `loadautosave` loads the latest after `stop`.

## Requirements

Java 17 or newer (the default image). About 512 MB of memory for a small server; more for large maps and many players. The installation is about 20 MB.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your settings, maps and saves:

```
# Ignore all
*
# Except the Mindustry settings, maps, mods and saves
!config/
# But not the logs
config/logs/
```
