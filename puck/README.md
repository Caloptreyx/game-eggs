# Puck

Puck is a physics-based multiplayer ice hockey game: skate, pass and shoot in fast team matches.

Native Calagopus egg for the Puck dedicated server (Steam app `3481440`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, password, player limit, port and public listing are written to `server_config.json` on every start; everything else in it stays yours.
- The server authenticates with Puck's backend on start and is listed when Public is on.
- Failed installations are reported to Calagopus as failed, with the reason.

The console shows the server log. The server reads no console input; admins (listed in `admin_steam_ids.json`) use the in-game commands. The panel's stop sends SIGINT, which ends the server at once; Puck keeps no world state.

## Server Ports

| Port | Default | Protocol  | Required |
|------|---------|-----------|----------|
| Game | 7777    | UDP + TCP | yes (primary allocation) |

## Configuration

| File | Contents |
|------|----------|
| `server_config.json` | Server settings: name, password, player limit, port, tick rate, public listing, voice chat, whitelist, mods, game mode, level |
| `public_game_mode_config.json` | Phase durations (warmup, play, intermission, ...), periods, spawn delay |
| `admin_steam_ids.json` | Admins, a JSON list of SteamID64 strings |
| `banned_steam_ids.json`, `banned_ip_addresses.json`, `whitelisted_steam_ids.json` | Bans and whitelist |

The server watches the admin, ban and whitelist files and applies changes while it runs; edit the two config files while the server is stopped.

## Requirements

About 265 MB of memory with no players; plan for 1 GB. The installation takes about 520 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Puck configuration, admins and bans
!*.json
```
