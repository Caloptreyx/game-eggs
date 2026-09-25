# American Truck Simulator

Drive across America in convoy with other players.

Native Calagopus egg for the official SCS Software dedicated server (Steam app `2239530`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Before the first start: server packages

The server needs two files exported from the game, which contain the map, DLCs and mods to use. Without them it refuses to start, and the console tells you so.

1. In the game's `config.cfg`, set `uset g_console "1"`.
2. Load a save, open the console with `~` and run `export_server_packages`.
3. Upload `server_packages.sii` and `server_packages.dat` from your game's user folder (`Documents/American Truck Simulator/`) to `data/American Truck Simulator/` on the server.

The files are not tied to your account. Re-export them when the game updates or your DLC/mod setup changes.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Lobby name, description, welcome message, password, max players, ports, logon token, player damage and traffic are written to `server_config.sii` on every start.
- The server keeps its home (config, packages, logs) in `data/American Truck Simulator/`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Connection | 27015 | TCP and UDP | yes (primary allocation) |
| Query      | 27016 | TCP and UDP | yes, for the server browser |

For LAN games the query port has to be between 27015 and 27020.

## Logon Token

Without a token the server logs in anonymously and gets a new search ID on every start. For a persistent ID, create a token at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with the game's app ID `270880` (not the server's), and enter it in Logon Token. Creating a token requires owning the game.

## Moderators

Add moderators by Steam ID in `data/American Truck Simulator/server_config.sii`, while the server is stopped:

```
 moderator_list: 2
 moderator_list[0]: 76561198000000001
 moderator_list[1]: 76561198000000002
```

Moderators can use `/set_time <HH:MM>` and `/set_rain_factor <0-1>` in chat.

## Requirements

About 1 GB of memory and 2 GB of disk. Up to 8 players.
