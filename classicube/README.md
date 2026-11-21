# ClassiCube

ClassiCube is a free, open source client compatible with Minecraft Classic: build with blocks in shared worlds, from a browser or the desktop client, on servers listed at classicube.net.

## MCGalaxy

Native Calagopus egg (`egg-classicube-mcgalaxy.yml`) for [MCGalaxy](https://github.com/ClassiCube/MCGalaxy), the most used ClassiCube server software. The installer builds the tagged release from source for .NET (the official download needs Mono) and installs the .NET 10 runtime into `.dotnet/` and the SQLite library MCGalaxy uses; it runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

### Capabilities

- Install of the latest release or a fixed tag (MCGalaxy Version); reinstall to update. Worlds (`levels/`), `properties/`, ranks, plugins and the database are kept.
- Port, server name, MOTD, player limit and classicube.net listing are written to `properties/server.properties` on every start; everything else in it stays yours.
- Console commands go straight to the server, e.g. `/players`, `/kick <name>`, `/rank <name> <rank>`, `/save all`, `/help`.
- The panel's stop sends SIGINT: the server saves changed worlds and shuts down cleanly.
- The server sends heartbeats to classicube.net and prints its play URL in the console; with Public off it stays unlisted, and players join through that URL or Direct connect.
- Failed installations are reported to Calagopus as failed, with the reason.

Players are verified against classicube.net accounts by default (`verify-names` in `server.properties`).

### Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 25565   | TCP      | yes (primary allocation) |

### Configuration

| Path | Contents |
|------|----------|
| `properties/server.properties` | Server settings: name, MOTD, port, limits, name verification, default world, ... |
| `properties/ranks.properties`, `ranks/` | Ranks, permissions, bans, whitelist |
| `levels/` | Worlds (`main.lvl` is the default), with backups in `levels/backups/` |
| `plugins/` | Compiled plugins (`.dll`) |

Edit the files while the server is stopped; most settings can also be changed in game by an owner-ranked player (`/server`, `/setrank`).

### Requirements

The panel measured about 17 MB of memory with no players; plan for 512 MB, more for many or large worlds. The installation takes about 92 MB of disk (most of it the .NET runtime).

### Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to worlds, settings and plugins:

```
# Ignore all
*
# Except worlds, settings, ranks, plugins and the database
!levels/
!properties/
!ranks/
!plugins/
!text/
!extra/
!MCGalaxy.db
```
