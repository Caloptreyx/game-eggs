# Garry's Mod

The physics sandbox.

Native Calagopus egg for the Source dedicated server (Steam app `4020`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- 32-bit public build by default, or the 64-bit build with Branch set to `x86-64` (reinstall after changing it). The 64-bit build can use more memory, which helps large servers.
- Workshop collections: set Workshop Collection to a collection ID; its addons are downloaded and mounted on start.
- Any gamemode and map, tickrate, and Lua refresh toggle.
- Server name, password, RCON password and loading screen URL are written to `garrysmod/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel gm_flatgrass`, `ulx ...`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and queries) |

## Game Server Login Token

Servers without a token only show up in the server list for LAN. Create one at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `4000` and enter it in Game Server Login Token.

## Gamemodes

Set Gamemode to the gamemode's folder name. Upload custom gamemodes to `garrysmod/gamemodes/`, or add them through the Workshop collection. For TTT, set Gamemode to `terrortown` and use a TTT map (e.g. from the Workshop).

## Files

| Path | Contents |
|------|----------|
| `garrysmod/cfg/server.cfg` | Server settings |
| `garrysmod/addons/` | Manually installed addons |
| `garrysmod/gamemodes/` | Gamemodes |
| `garrysmod/data/` | Addon data |
