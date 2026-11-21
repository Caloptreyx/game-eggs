# Rising World

Rising World is an open-world sandbox and survival game. Players explore an endless procedural world, mine, craft, farm, hunt and tame animals, and build freely or block by block, alone or together.

Native Calagopus egg for the dedicated server of the current (Unity) version of Rising World (Steam app `339010`), installed anonymously through SteamCMD. The Java Legacy version has its own egg, described at the end.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, password, player limit, visibility, world name, game mode and seed are written to `server.properties` on every start; everything else in it stays yours. The file is created once from the shipped `server.example.properties`.
- Console commands work in the panel console.
- The panel's stop makes the server save the world and exit (`[END]` in the log).
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads its console like a terminal: it needs a sized pseudo-terminal and only acts on an Enter once more input follows. The startup command therefore runs it under `script` and forwards each console line through `.console.fifo` with a CR and an empty Enter. On stop the shell catches SIGINT and sends `shutdown` until the server has exited; a plain signal would kill it without saving.

Worlds are SQLite databases in `Worlds/<World Name>/`; logs are in `Logs/`. Game mode and seed only apply to a new world; change World Name to create one. Java plugins go into `Plugins/`.

## Server Ports

Rising World derives the Steam query port from the game port, so the server needs two consecutive allocations with the higher one as the primary:

| Port  | Default | Protocol | Notes |
|-------|---------|----------|-------|
| Game  | 4255    | UDP      | primary allocation |
| Query | 4254    | UDP/TCP  | game port - 1; Steam server browser (A2S) and the server's web server |
| RCON  | 4253    | TCP      | only with `RCON_Enabled=True` in `server.properties`; needs its own allocation |

## Java Legacy egg

`egg-rising-world-legacy.yml` runs the original Java version from the `java` ("Legacy Version") branch of app 339010 on the `ghcr.io/caloptreyx/java:8` image. That branch has not changed since 2021, so the egg installs it once (no update on start).

- Server name, player limit, game port and RCON port are launch arguments; game password, RCON settings and the world name are written to `server.properties` on every start (the server adds all other settings on its first start). Worlds are saved in `Worlds/<World Name>/`.
- The server uses the game port and the three ports after it (TCP and UDP), so give it four consecutive allocations with the lowest as the primary; Steam queries (A2S) answer on the game port. RCON needs a further allocation.
- Its Steamworks wrapper loads `steamclient.so`, which the installer puts into `.steam/sdk64` and the startup adds to `LD_LIBRARY_PATH`.
- Console commands work in the panel console; the panel's stop sends `shutdown`, which saves the world and stops the server.

It needs about 350 MB of memory with a new world and no players and about 2.7 GB of disk.

## Requirements

About 700 MB of memory with a new world and no players; plan for 4 GB. The installation takes about 2.9 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds, plugins and settings:

```
# Ignore all
*
# Except the worlds, plugins and settings
!Worlds/
!Plugins/
!server.properties
```
