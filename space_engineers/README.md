# Space Engineers

Space Engineers is a sandbox game about engineering, construction, exploration and survival in space and on planets: build ships, stations and planetary outposts, mine resources and fight, alone or together.

Native Calagopus egg for the Space Engineers dedicated server (Steam app `298740`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton (on Proton's built-in .NET) in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, world, world template, game mode, max players, password and ports are written to the server config on every start.
- The server log is streamed to the console. The server reads no console input.
- Stopping the server saves the world first (through the server's local Remote API), then shuts it down.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 27016   | UDP      | yes (primary allocation); also answers Steam server queries (A2S) |
| Steam | 8766    | UDP      | additional allocation set as Steam Port; the test server did not open it |

The Remote API (TCP 8080) only listens on 127.0.0.1 inside the container and needs no allocation; the egg uses it to save and stop the server.

## Configuration

Config, worlds and logs are kept in `instance/`. `instance/SpaceEngineers-Dedicated.cfg` is created by the installer; on every start the panel writes the Server Name, World Name, World Template, Game Mode, Max Players, the ports and the Remote API settings into it, and the Server Password is stored there as the salted hash the server expects. Other settings (session settings such as inventory size, assembler speed, enemies, ...) are edited in that file while the server is stopped.

World Template and Game Mode only apply when the world named World Name does not exist yet: a new world is created from the template on the first start.

## Saves

Worlds are stored in `instance/Saves/<World Name>/`, with backups in its `Backup/` folder. The server saves when it is stopped from the panel and at the autosave interval set in the config. To use an existing world, stop the server, upload its folder to `instance/Saves/` and set World Name to the folder name. Older server logs are moved to `instance/Logs/`.

## Requirements

About 2.3 GB of memory with a new Star System world and no players; plan for 6-8 GB, more with large grids and many players. The installation takes about 8.7 GB of disk; a new world about 1.3 MB.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and config:

```
# Ignore all
*
# Except the Space Engineers worlds and config
!instance/
```
