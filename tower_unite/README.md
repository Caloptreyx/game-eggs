# Tower Unite

Tower Unite is a social game world: hang out in the Plaza, play arcade minigames such as Ball Race, Minigolf, Virus and Zombie Massacre, and visit player condos.

Native Calagopus egg for the Tower Unite dedicated server (Steam app `439660`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server title, player limit, Steam login token and admin are written to `Tower/Saved/Config/TowerServer.ini` on every start; everything else in it stays yours.
- The server log is shown in the console. The server reads no console input; the panel's stop sends SIGINT.
- Failed installations are reported to Calagopus as failed, with the reason.

The Steam query answers with the server's Steam ID as its name; the title is in the query rules (`ServerTitle_s`), which Tower Unite's server list shows. Player progress is stored by the game, not on the server.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam server queries) |

## Game Server Login Token

To appear in the public server list, create a token at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `394690` (Tower Unite) and enter it in Game Server Login Token.

## Configuration

`Tower/Saved/Config/TowerServer.ini` holds the server options (`[/Script/TowerNetworking.DedicatedServerOptions]`: game mode, map and the other dedicated server options from the game's server documentation) and `[Administration]`. Edit it while the server is stopped.

## Requirements

About 2.4 GB of memory with no players on the Plaza; plan for 4 GB. The installation takes about 4.9 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the server configuration
!Tower/Saved/Config/
```
