# Solace Crafting

Solace Crafting is an open-world survival crafting RPG. Players explore a procedurally generated fantasy world, gather and craft, build bases and fight creatures, alone or together.

Native Calagopus egg for the Solace Crafting dedicated server (Steam app `1086950`), installed anonymously through SteamCMD.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Name, description, ports, visibility, password, player limit, admin settings and the world save are written to `.config/unity3d/Big Kitty Games/Solace Crafting/servercfg.dat` (JSON) on every start. The installer seeds the file; moderator settings and `autoRestart` stay yours.
- The Unity log is shown in the console. The server reads no console input.
- The panel's stop sends SIGINT; the server disconnects players, stops (`Server: stopping...`) and exits.
- Failed installations are reported to Calagopus as failed, with the reason.

Worlds are kept in `.config/unity3d/Big Kitty Games/Solace Crafting/Worlds/<World Save>/`; a new World Save name creates a new world.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 27015   | UDP      | yes (primary allocation) |
| Query | 27016   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |

## Requirements

About 930 MB of memory with a new world and no players; plan for 3 GB. The installation takes about 1.2 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and settings:

```
# Ignore all
*
# Except the worlds and settings
!.config/
```
