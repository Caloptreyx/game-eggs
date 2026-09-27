# Satisfactory

Build huge factories, explore an alien planet and automate everything, alone or with friends.

Native Calagopus egg for the Satisfactory dedicated server (Steam app `1690800`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), stable or experimental branch.
- Max players set from the panel on every start.
- The console runs server commands through the server's HTTPS API and prints their output, e.g. `help`, `server.SaveGame <name>`, `FG.AutosaveInterval 600`, `server.GenerateAPIToken`.
- Stopping the server saves the running session first. The server itself does not save on shutdown, so `quit`, `stop` and `exit` from the console (and the panel's stop button) save it as `<session>_shutdown` before shutting down.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port     | Default | Protocol  | Required |
|----------|---------|-----------|----------|
| Game     | 7777    | UDP + TCP | yes (primary allocation); TCP carries the HTTPS API |
| Reliable | 8888    | TCP       | yes |

The game port cannot be remapped: the external port must match the allocation. Players add the server in the Server Manager with the primary port only; the game tells them the reliable port.

## Setup

The server is claimed and configured in game:

1. Start the server and wait until it is ready.
2. In the game's main menu open the Server Manager, add the server (`<ip>:<game port>`) and accept the self-signed certificate.
3. Set a server name and an admin password, then create a new game or upload a save.

Server name, admin and player passwords, the session to load on start, auto-pause, autosave interval and network quality are all set in the Server Manager (or through the HTTPS API) and stored by the server. To reset the admin password, stop the server and delete `.config/Epic/FactoryGame/Saved/SaveGames/ServerSettings.<game port>.sav`; this also resets the name, player password and certificate.

## Configuration

Max Players is passed to the server on every start (`-ini:Game:[/Script/Engine.GameSession]:MaxPlayers=`). The game is tuned for 4 players.

Other engine settings go in `FactoryGame/Saved/Config/LinuxServer/Engine.ini` and `Game.ini`, edited while the server is stopped (the server rewrites them on shutdown), or as `-ini:<File>:[<Section>]:<Key>=<Value>` under Extra Arguments. See the [configuration files](https://satisfactory.wiki.gg/wiki/Dedicated_servers/Configuration_files) page for known settings.

### Console and HTTPS API

The server reads no input from its terminal. The panel console sends each line to the local HTTPS API as a console command; for that the server is started with `FG.DedicatedServer.AllowInsecureLocalAccess=1`, which lifts authentication only for connections from inside the container (127.0.0.1). Remote API access still needs a token: `server.GenerateAPIToken` in the console prints one to use as `Authorization: Bearer <token>`.

The console is available once the server is ready; while a save is loading the API does not answer and the command is dropped. Console output is the command result only; the server also logs to the console.

## Saves

`HOME` is the server directory, so everything the server keeps in `~/.config/Epic/FactoryGame/` stays with the server files:

- Saves: `.config/Epic/FactoryGame/Saved/SaveGames/server/`
- Server settings (name, passwords, certificate): `.config/Epic/FactoryGame/Saved/SaveGames/ServerSettings.<game port>.sav`
- Blueprints: `.config/Epic/FactoryGame/Saved/SaveGames/blueprints/`

The server autosaves every 5 minutes by default (Autosave Interval in the Server Manager, or `FG.AutosaveInterval <seconds>` in the console) and keeps 3 rotating autosaves. It does not save when it shuts down: the panel's stop button saves `<session>_shutdown` first, but a kill (or a crash) loses everything since the last save. When a save is overwritten, the server keeps the previous file as `<name>-<date>.sav` (up to 25 backups). With the session set as Auto-Load Session, the server loads the newest save of it on the next start. Logs are in `FactoryGame/Saved/Logs/`.

## Requirements

About 0.5 GB of memory with no game loaded and 3 GB with a new game; usage grows with the factory. Plan for 8 GB, 16 GB for large factories or more than 4 players. The server favours high single-core CPU performance. The installation takes about 3 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your config and saves:

```
# Ignore all
*
# Except the Satisfactory saves, server settings and config
!.config/Epic/FactoryGame/
!FactoryGame/Saved/Config/
```
