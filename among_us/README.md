# Among Us

Among Us is a social deduction game: crewmates finish tasks on a spaceship while impostors sabotage and eliminate them.

## Impostor

Native Calagopus egg for [Impostor](https://github.com/Impostor/Impostor), the open source private server for Among Us. The official Linux release is downloaded from GitHub together with the ASP.NET Core runtime it needs; it runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

### Capabilities

- Install of the latest Impostor release or a fixed tag (Impostor Version); reinstall to update. `config.json` and `plugins/` are kept.
- Port and public address come from the panel on every start (as `IMPOSTOR_*` environment variables, which override `config.json`); the console shows the address players are sent to.
- Anti-cheat on/off from the panel.
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads no console input. The panel's stop sends SIGINT and the server exits cleanly; Impostor keeps no game state between restarts.

### HTTPS

Since Among Us 16.0.5 players can only connect to servers that support HTTPS. Impostor itself serves plain HTTP on the panel port, so for current game versions put a reverse proxy with a TLS certificate in front of it (outside the panel) and give players the HTTPS address. See Impostor's [HTTP server documentation](https://github.com/Impostor/Impostor/blob/master/docs/Http-server.md).

### Server Ports

| Port | Default | Protocol  | Required |
|------|---------|-----------|----------|
| Game | 22023   | UDP + TCP | yes (primary allocation; UDP for games, TCP for the HTTP matchmaker) |

### Public IP

Players are sent to the Public IP variable. When it is empty the allocation's IP is used, or the server's public IP (looked up from api.ipify.org on start) when the allocation listens on `0.0.0.0`.

### Configuration

`config.json` holds the remaining settings: anti-cheat details, timeouts, compatibility (future game versions, version mixing, host authority) and the game recorder. Plugins go into `plugins/`, their dependencies into `libraries/`.

### Requirements

The running server measured about 21 MB of memory with no lobbies; plan for 256 MB. The installation takes about 110 MB of disk, most of it the .NET runtime.

### Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and plugins:

```
# Ignore all
*
# Except the configuration and plugins
!config.json
!plugins/
!libraries/
```

## BetterCrewLink server

The BetterCrewLink voice relay from the Pterodactyl collection is not ported yet.
