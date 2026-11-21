# Romestead

Rome has fallen! Rebuild civilization in this action-adventure survival game for 1-8 players. Fight hordes of the walking dead, build settlements, restore the Roman gods... or kick back and farm with friends.

Native Calagopus egg for the Romestead dedicated server (Steam app `4763510`). The server is a cross-platform .NET 8 application and runs natively on Linux in the `ghcr.io/caloptreyx/steamcmd:debian` image; the .NET 8 runtime is installed with the server files.

## Capabilities

- Server update on every start (Auto Update), optionally from a beta branch.
- World name, size, seed, password, max players and port are written to `config.json` on every start.
- The console accepts the server's commands (see below).
- Stopping the server saves the world first.
- Failed installations are reported to Calagopus as failed, with the reason.

## Installation and updates

SteamCMD refuses to download this server anonymously ("Not for anonymous users"), but Steam serves its files to anonymous [DepotDownloader](https://github.com/SteamRE/DepotDownloader) logins, so the egg installs and updates the server with DepotDownloader (kept in `.depotdownloader/`). No Steam account is needed. The .NET 8 runtime is installed into `.dotnet/`; reinstall the server to update it.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 8050    | UDP      | yes (primary allocation); any port works |

Players join with the server's IP and port.

## Configuration

`config.json` in the server root is rewritten on every start from the panel variables (`AutoStartWorldName`, `AutoCreateAndLoadWorld`, `AutoCreateWorldSize`, `AutoCreateWorldSeed`, `Password`, `Port`, `MaxPlayers`); other keys in it are kept. The server loads the world named World Name and creates it with World Size and World Seed when it does not exist. World Size and World Seed only apply to new worlds.

## Console commands

| Command | Description |
|---------|-------------|
| `stop` | Saves the world, then stops the server (the panel's stop button sends it) |
| `quickstop` | Stops the server without saving |
| `save` | Full save; the server is frozen during the save |
| `quicksave` | Saves on a second thread while the server keeps running |
| `list` | Lists the connected players |
| `say <message>` | Sends a chat message to all players |
| `kick <ip>` / `ban <ip>` / `unban <ip>` | Kicks, bans or unbans a player by IP; bans last until the next server start |

## Saves

Worlds are stored in `saved_worlds/<World Name>/` (`game_state`, `world_desc`). Each save keeps the previous one as `game_state_backup` / `world_desc_backup`, which the server restores automatically if the latest save fails to load. The server autosaves every 6 minutes and on `stop`.

## Requirements

Generating a world takes the most memory: about 2.6 GB for a Small and 4 GB for a Standard world (the Romestead wiki asks for 8 GB to generate a Large one). .NET keeps that memory afterwards while the host has it free; a restarted server with an existing Standard world and no players used about 0.8 GB. Plan for 4 GB, 8 GB for Large worlds or 5+ players. The installation (server, .NET runtime and DepotDownloader) takes about 400 MB of disk; a Standard world about 55 MB with its backup.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and the config:

```
# Ignore all
*
# Except the Romestead worlds and config
!saved_worlds/
!config.json
```
