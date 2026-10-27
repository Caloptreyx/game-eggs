# Team Fortress 2

The class-based team shooter.

Native Calagopus egg for the Source dedicated server (Steam app `232250`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- 64-bit server (`srcds_run_64`), falling back to the 32-bit `srcds_run` if the 64-bit build is missing.
- Any map, player count, and extra launch parameters or convars (e.g. `+tf_bot_quota 8`).
- Server name, password and RCON password are written to `tf/cfg/server.cfg` on every start. The rest of the file stays yours.
- Console commands go straight to the server, e.g. `status`, `changelevel pl_badwater`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP and TCP | yes (primary allocation; also used for RCON and queries) |

## Game Server Login Token

Servers without a token only show up in the server list for LAN. Create one at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `440` and enter it in Game Server Login Token.

## Files

| Path | Contents |
|------|----------|
| `tf/cfg/server.cfg` | Server settings |
| `tf/cfg/mapcycle.txt` | Map rotation (falls back to `mapcycle_default.txt`) |
| `tf/custom/` | Custom content |
| `tf/addons/` | Server plugins (Metamod:Source, SourceMod) |

## Stopping

Stopping sends `quit`; the server shuts down in a couple of seconds. srcds aborts at the very end of its shutdown (Steam client assertions in the console); that is harmless, and the egg turns core dumps off so it leaves no core files behind. TF2 keeps no world saves.

## Requirements

- About 15 GB of disk.
- About 1 GB of RAM for an empty server.

## Sample `.pteroignore`

```
steamcmd/
steamapps/
bin/
hl2/
platform/
```
