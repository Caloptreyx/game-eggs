# Killing Floor 2

Co-op survival shooter: up to six players fight waves of Zeds and a boss across a ruined Europe, with Versus, Weekly Outbreak, Endless and Objective modes.

Native Calagopus egg for the Killing Floor 2 dedicated server (Steam app `232130`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Map, game mode, difficulty, game length and max players are passed on the command line on every start.
- Server name, game password, admin password, WebAdmin and server takeover are written to the config files on every start. The installer starts the server once to generate those files, so the panel settings apply from the first start on.
- Console commands go through WebAdmin's management console and their output is printed in the panel console, e.g. `getall KFGameInfo GameLength`, `kick <name>`, `changemap KF-Outpost`; `say <message>` posts to the in-game chat. This needs WebAdmin on and an Admin Password.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game     | 7777  | UDP | yes (primary allocation) |
| Query    | 27015 | UDP | yes, for the server browser and Steam queries |
| WebAdmin | 8080  | TCP | yes, for WebAdmin from a browser (the panel console uses it locally) |

The server also connects out to Steam on port 20560 (UDP); that needs no allocation.

## Configuration

The config files are generated in `KFGame/Config/`:

| File | Contents |
|------|----------|
| `LinuxServer-KFGame.ini` | Gameplay: name (`[Engine.GameReplicationInfo]`), passwords (`[Engine.AccessControl]`), map cycle (`GameMapCycles`), MOTD and banner, ... |
| `LinuxServer-KFEngine.ini` | Network, takeover (`bUsedForTakeover`), Workshop items (`[OnlineSubsystemSteamworks.KFWorkshopSteamworks]`) |
| `KFWeb.ini` | WebAdmin on/off and port |
| `KFWebAdmin.ini` | WebAdmin options, multiple admin accounts |

Edit them only while the server is stopped. The panel-managed settings are rewritten on every start, so change those in the panel. Do not edit the `Default*.ini` files.

Game modes: `KFGameContent.KFGameInfo_Survival` (6 players), `KFGameContent.KFGameInfo_VersusSurvival` (12 players), `KFGameContent.KFGameInfo_WeeklySurvival`, `KFGameContent.KFGameInfo_Endless`, `KFGameContent.KFGameInfo_Objective`.

### Workshop maps

Add `ServerSubscribedWorkshopItems=<id>` lines to a `[OnlineSubsystemSteamworks.KFWorkshopSteamworks]` section in `LinuxServer-KFEngine.ini` and `DownloadManagers=OnlineSubsystemSteamworks.SteamWorkshopDownload` as the first `DownloadManagers=` line under `[IpDrv.TcpNetDriver]`. The server downloads them into `KFGame/Cache/` on start (the installer creates that folder, which Linux servers do not create themselves).

## Admins

Log in to WebAdmin at `http://<server ip>:<WebAdmin port>/` with user `Admin` and the Admin Password. In game, admins open the console and enter `AdminLogin <password>`. For several WebAdmin accounts see `KFWebAdmin.ini` (`AuthenticationClass=WebAdmin.MultiWebAdminAuth`).

## Saves

Killing Floor 2 keeps no world state: every match starts fresh, and player progress is stored by Steam on the client. The server stops right away on the stop signal; there is nothing to save. Only the config files under `KFGame/Config/` are worth backing up.

## Requirements

About 0.4 GB of memory on KF-BioticsLab with no players; more with players, larger maps and Workshop content. Plan for 2 GB. The installation takes about 31 GB of disk, so give the server at least 35 GB: the server does not start while the disk limit is exceeded.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Killing Floor 2 config
!KFGame/Config/
```
