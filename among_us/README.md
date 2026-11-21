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

Native Calagopus egg (`egg-bettercrewlink-server.yml`) for the voice server of [BetterCrewLink](https://github.com/OhMyGuus/BetterCrewLink), the proximity voice chat mod for Among Us. The installer downloads the [server source](https://github.com/OhMyGuus/BetterCrewLink-server) (Git Branch, default `master`), installs Node.js (Node.js Version, default 22) into `.node/` and builds the server; it runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

### Capabilities

- Server name and port from the panel on every start; `http://<address>:<port>/health` shows them with the connection and lobby counts.
- HTTPS switch: with HTTPS on, the server reads `privkey.pem` and `fullchain.pem` from the server root.
- Reinstall to update the source or change the Node.js version; `config/peerConfig.yml` and the certificates are kept.
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads no console input. The panel's stop sends SIGINT and the server exits at once; it keeps no state.

### Server Ports

| Port   | Default | Protocol | Required |
|--------|---------|----------|----------|
| Server | 9736    | TCP      | yes (primary allocation; players enter `http://<address>:<port>` as voice server in BetterCrewLink) |

### Peer connections

Voice runs peer to peer; by default clients use Google's STUN server. To relay voice (for players behind strict NAT), copy `config/peerConfig.example.yml` to `config/peerConfig.yml` and enable the built-in TURN relay or add your own TURN servers. The built-in relay listens on UDP 3478 and relays through UDP ports 49152-65535 unless changed there; those ports need allocations, so for a panel server set `listeningPort`, `minPort` and `maxPort` to allocated ports, and set the Public Address variable to the server's public IP or DNS name.

### Requirements

The panel measured about 27 MB of memory with no lobbies; plan for 256 MB. The installation takes about 290 MB of disk (Node.js, dependencies and the build).

### Sample ignore file for backups

```
# Ignore all
*
# Except the relay configuration and the certificates
!config/peerConfig.yml
!*.pem
```
