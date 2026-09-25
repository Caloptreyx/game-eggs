# ARK: Survival Evolved

Stranded on the shores of a mysterious island called ARK: hunt, harvest, craft, build and tame dinosaurs to survive.

Native Calagopus egg for the Linux dedicated server (Steam app `376030`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image. For ARK: Survival Ascended, use the separate `ark_survival_ascended` egg.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, passwords, PvE and max players are written to `GameUserSettings.ini` by the panel, so names may contain quotes, `$` and brackets.
- The server log is streamed to the console, and console commands are sent over RCON once the server is up, e.g. `ListPlayers`, `SaveWorld`, `Broadcast Hello`.
- Stopping the server sends `DoExit`, which saves the world before exiting.
- Steam Workshop mods by ID, downloaded and updated by the server itself (`-automanagedmods`).
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game  | 7777  | UDP | yes (primary allocation); the game also uses the next port (7778, UDP) |
| Query | 27015 | UDP | yes, for the Steam server list |
| RCON  | 27020 | TCP | only for remote admin tools; the panel console uses it locally |

Add the port after the game port (7778 by default) as an allocation too.

## Configuration

| File | Contents |
|------|----------|
| `ShooterGame/Saved/Config/LinuxServer/GameUserSettings.ini` | Server settings (`[ServerSettings]`, rates, ...) |
| `ShooterGame/Saved/Config/LinuxServer/Game.ini` | Advanced gameplay settings |

The server rewrites these files when it stops, so edit them only while it is stopped. The panel-managed settings are written on every start, so change those in the panel.

## Startup time

A first start takes several minutes, much longer with mods to download. The panel's stop goes through RCON, so it only works once the server is up.

## Requirements

About 6 GB of memory for an empty server on The Island; DLC maps need more (Genesis Part 2 over 13 GB). About 30 GB of disk, more with mods.
