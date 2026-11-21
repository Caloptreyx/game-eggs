# RuneScape: Dragonwilds

On RuneScape's forgotten continent of Ashenfall, dragons have awoken. Gather, build, skill and craft in this co-operative survival adventure. Only by mastering survival and uncovering ancient secrets can you hope to face the ferocious Dragon Queen.

Native Calagopus egg for the RuneScape: Dragonwilds dedicated server (Steam app `4019830`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Owner, server name, world name, passwords and admins are written to `DedicatedServer.ini` on every start.
- The console shows the server log. The server reads no console input; the world is managed in game.
- Stopping the server finishes pending saves before it exits.
- Failed installations are reported to Calagopus as failed, with the reason.

## Required settings

- **Owner ID:** your RuneScape: Dragonwilds Player ID (32 letters and digits), shown in game at the bottom of the Settings menu. The server does not start without an owner.
- **Admin Password:** players who enter it in Pause Menu > Settings > Server Management become admins.

## Server Ports

| Port   | Default | Protocol | Required |
|--------|---------|----------|----------|
| Game   | 7777    | UDP      | yes (primary allocation) |
| Beacon | 8888    | UDP      | yes, its own allocation (Beacon Port variable) |

## Joining

Players find the server in the Public tab of the Worlds screen by searching the **World Name** (case sensitive). The world list connects to the server's public address; players on the same network as the server use the Direct tab with the server's LAN address and game port instead.

## Configuration

`RSDragonwilds/Saved/Config/LinuxServer/DedicatedServer.ini` holds the server settings (section `[/Script/Dominion.DedicatedServerSettings]`). The panel writes `OwnerId`, `ServerName`, `DefaultWorldName`, `WorldPassword`, `AdminPassword` and `AdministratorList` into it before every start. The server rewrites the file while it runs: it adds `ServerGuid`, `PlatformPolicy` and other settings, and removes `AdminPassword` once it has read it (the panel writes it again on the next start). Edit other values only while the server is stopped.

## Users and admins

| Role | How | Rights |
|------|-----|--------|
| Owner | Player ID equals Owner ID | Can ban and unban anyone, online or offline |
| Admin | Listed in Admins, or entered the Admin Password in game | Can ban online regular players, cannot unban |
| Player | Knows the World Name (and World Password) | No moderation rights |

## Saves

The world is saved as `RSDragonwilds/Saved/SaveGames/<World Name>.sav`. On start the server loads the save of the configured World Name, or creates a new Standard world with that name. To use a world made in the game (e.g. with custom settings), stop the server, upload its `.sav` to `RSDragonwilds/Saved/SaveGames/` **without renaming it**, and set World Name to the file name without `.sav`.

The server saves in response to changes (a player's state always when they log out). The panel's stop sends SIGINT: the server finishes pending saves and exits cleanly.

## Requirements

The server used about 1 GB of memory with a new world and no players. Jagex's official container asks for at least 4 GB, and the RuneScape: Dragonwilds Wiki gives 2 GB plus 1 GB per player; this build allows at most 6 players, so plan for 8 GB. The installation takes about 5.5 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the saves and the config:

```
# Ignore all
*
# Except the Dragonwilds saves and config
!RSDragonwilds/Saved/SaveGames/
!RSDragonwilds/Saved/Config/
```
