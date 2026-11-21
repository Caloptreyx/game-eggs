# Grand Theft Auto Connected

[GTA Connected](https://gtaconnected.com/) is a multiplayer mod for Grand Theft Auto III, Vice City, San Andreas and IV, with server-side scripting in JavaScript, Lua and Squirrel.

Native Calagopus egg for the official GTA Connected Linux server from gtaconnected.com. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image. Players need the game they join and the GTA Connected client.

## Capabilities

- Install of the newest Linux server or a fixed version (GTA Connected Version); reinstall to update. `server.xml` and `resources/` are kept.
- Server name, game, game mode name, player limit, password, port and server browser listing are written to `server.xml` on every start; everything else in it stays yours.
- The server registers with the GTA Connected server listing, which pings it back before adding it.
- Console commands go straight to the server ([server commands](https://wiki.gtaconnected.com/)); stopping sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol  | Required |
|------|---------|-----------|----------|
| Game | 22000   | UDP + TCP | yes (primary allocation; UDP for the game, TCP for the built-in HTTP server that serves resources to clients) |

## Configuration

`server.xml` holds the server settings ([options](https://wiki.gtaconnected.com/ServerConfiguration)): streaming and sync distances, logging, modules, global cvars (traffic, civilians), rules shown in the browser, and the resources to start. Resources (game modes and scripts) go into `resources/<name>/` with a `meta.xml`; the shipped `freeroam` resource is started by default. GTA IV servers also read `iv_gamemode` and `iv_episode`.

## Requirements

About 10 MB of memory with no players; plan for 256 MB. The installation takes about 40 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and resources:

```
# Ignore all
*
# Except the configuration and resources
!server.xml
!resources/
```
