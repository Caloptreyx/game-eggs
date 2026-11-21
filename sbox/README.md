# s&box

s&box is Facepunch's game platform built on Source 2: its dedicated server runs any s&box game package (Sandbox, or community games published on sbox.game) with an optional map package.

Native Calagopus egg for the s&box dedicated server (Steam app `1892930`), installed anonymously through SteamCMD. It uses the **native Linux build**, which ships its own .NET runtime, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from the `staging` branch.
- Game package, map package, server name, Steam relay and query port are set from the panel; further console variables through Extra Arguments.
- The game and map packages are downloaded from sbox.game when the server starts, so a new version of the game is picked up on the next start.
- Failed installations are reported to Calagopus as failed, with the reason.

The console shows the server log but takes no commands: the server only reads input through an interactive terminal overlay, which crashes in the panel console, so it is turned off.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 27015   | UDP      | yes (primary allocation); used by players only with Steam Relay off |
| Query | 27016   | UDP      | yes, additional allocation set as Query Port (Steam server list, A2S) |

With Steam Relay on (the default), players connect through Steam's relay network, which hides the server's IP address and needs no open game port.

## Configuration

- **Game**: package ident from the game's sbox.game page, e.g. `facepunch.sandbox`; append `#<version>` to pin a version.
- **Map**: package ident of a map, e.g. `facepunch.flatgrass`; empty loads the game's default scene.
- **Game Server Login Token**: optional Steam token (app ID 590830) so the server keeps the same Steam ID across restarts; without it the server gets a new anonymous ID on every start.
- **Extra Arguments**: further console variables, e.g. `+net_max_outgoing 2048`.

## Saves

The server keeps no world of its own; whatever a game saves is up to that game. Downloaded packages are cached in `download/` and `addons/`, server logs are written to `logs/`.

## Stopping

The server has no shutdown handling of its own: the panel's stop signal ends it at once.

## Requirements

About 1.3-1.7 GB of memory running Sandbox on Flatgrass with no players; plan for 4 GB, more for heavier games. The installation takes about 4.7 GB of disk with the Sandbox packages downloaded.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this leaves out the largest folders, which SteamCMD installs again and the server downloads again on start:

```
# Server files and package downloads
bin/
core/
addons/
download/
steamcmd/
```
