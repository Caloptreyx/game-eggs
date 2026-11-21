# open.mp

open.mp (Open Multiplayer) is the multiplayer server for Grand Theft Auto: San Andreas that replaces the SA-MP server: it runs SA-MP game modes, filterscripts and most plugins, and SA-MP 0.3.7 / 0.3.DL clients and the open.mp launcher can join. Players need their own copy of GTA: San Andreas; the server does not.

Native Calagopus egg for the official open.mp Linux server (`open.mp-linux-x86` from the open.mp GitHub releases). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Installs the latest release or a fixed release tag (open.mp Version). Reinstall to update; `config.json`, game modes, filterscripts, scriptfiles, plugins and bans are kept.
- Server name, password, player limit, port, public listing and rcon are written to `config.json` on every start; everything else in it (game modes, scripts, plugins, ...) stays yours.
- Console commands go straight to the server, e.g. `gmx`, `changemode <mode>`, `kick <id>`, `ban <id>`, `say <text>`. Stopping the server sends `exit`.
- Failed installations are reported to Calagopus as failed, with the reason.

The server is a 32-bit program; the installer adds the 32-bit `libatomic` it needs in `lib32/`.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 7777    | UDP      | yes (primary allocation; also answers SA-MP server queries) |

## Configuration

`config.json` holds all settings. The main game mode is set under `pawn.main_scripts` (the default is the bundled `gungame` example; put your compiled `.amx` in `gamemodes/`), filterscripts under `pawn.side_scripts`, legacy SA-MP plugins under `pawn.legacy_plugins` (Linux `.so` builds in `plugins/`). Edit it while the server is stopped. The Pawn compiler and include files are in `qawno/`.

## Saves

open.mp keeps no world state of its own; whatever a game mode stores goes to `scriptfiles/` (or its database). Bans are in `bans.json`, the server log in `log.txt`.

## Requirements

About 50 MB of memory with the bundled example game mode and no players; plan for 256-512 MB, more for large game modes. The installation takes about 120 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this leaves out the server binary and components, which a reinstall restores:

```
components/
omp-server
lib32/
qawno/
```
