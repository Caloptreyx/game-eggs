# Luanti

Luanti (formerly Minetest) is an open source voxel game engine: run Minetest Game, VoxeLibre, Mineclonia or any of the hundreds of games and mods on ContentDB.

Native Calagopus egg for the headless Luanti server (`luantiserver`), which is part of the `ghcr.io/caloptreyx/games:luanti` image (built from the Luanti release; the image is updated to ship new engine versions). The installer downloads the game from ContentDB.

## Capabilities

- Any game from ContentDB (Game Package, e.g. `Luanti/minetest_game`, `Wuzzy/mineclone2` for VoxeLibre, `ryvnf/mineclonia`); reinstall to install another game or update it.
- Server name, description, message of the day, player limit, default password, admin name, server list announcement, damage and creative mode are written to `luanti/luanti.conf` on every start; other settings in that file stay yours.
- The panel's stop sends SIGINT: the server saves the world and exits.
- Failed installations are reported to Calagopus as failed, with the reason.

The console shows the server log. The server reads no console input; the player named Admin Name has all privileges and administers the server with chat commands (`/grant`, `/kick`, `/ban`, `/shutdown`, ...).

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 30000   | UDP      | yes (primary allocation) |

## Configuration

Luanti's user folder is `luanti/` in the server root: `luanti/games/` (the installed game), `luanti/mods/` (mods; enable them per world in `luanti/worlds/<world>/world.mt` with `load_mod_<name> = true`), `luanti/worlds/` and `luanti/luanti.conf` (all server settings; see the engine's `minetest.conf.example`). Game ID must match the game's folder name in `luanti/games/` (the installer prints it). A new World Name starts a new world with the current game.

## Saves

Worlds are stored in `luanti/worlds/<World Name>/` (`map.sqlite`, `players.sqlite`, `auth.sqlite`, `world.mt`, ...). The server saves continuously and when it is stopped from the panel. The debug log is `luanti/debug.txt`.

## Requirements

About 20 MB of memory with Minetest Game, a new world and no players; memory grows with players, loaded map area and mods. Plan for 512 MB-2 GB. The server files (game and a new world) take about 11 MB of disk; worlds grow as they are explored.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds, mods and settings:

```
# Ignore all
*
# Except the Luanti worlds, mods and settings
!luanti/worlds/
!luanti/mods/
!luanti/luanti.conf
```
