# SuperTuxKart

SuperTuxKart is a free and open source 3D kart racer: races, time trials, battles, soccer and capture the flag with Tux and other open source mascots.

Native Calagopus egg for a SuperTuxKart server run from the official Linux release ([GitHub releases](https://github.com/supertuxkart/stk-code/releases)) as a headless server with its bundled libraries; nothing is compiled. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Install of the latest release or a fixed tag (SuperTuxKart Version); reinstall to update. `server_config.xml` and the player data in `.config/` and `.local/` are kept.
- Server name, password, player limit, port and message of the day are passed on every start and saved into `server_config.xml`.
- Network console: `listpeers`, `kick <id>`, `kickban <id>`, `listban`, `speedstats`, `kickall`, `help`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

SuperTuxKart keeps no world state.

## LAN server and online list

The egg starts a LAN server. Players outside the local network join with **Online → Enter server address** and `<ip>:<port>`. Listing the server on the official online server list (a WAN server) needs a registered stk-addons account whose session is saved on the server (`--init-user --login=<name> --password=<password>`, then `--wan-server=<name>` instead of `--lan-server`); the egg does not do this.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 2759    | UDP      | yes (primary allocation) |

## Configuration

`server_config.xml` in the server root is created on the first start, with a comment for every setting: game mode (`server-mode`: 3 normal race, 4 time trial, 6 soccer, 7 free-for-all, 8 capture the flag; 0 and 1 the grand prix variants), difficulty, laps, owner-less mode, voting, chat, ping limits and more. Edit it while the server is stopped; the panel's values are written into it on every start.

## Requirements

The panel measured under 20 MB of memory with no players; plan for 512 MB for races. The installation takes about 820 MB of disk (the server needs the game's tracks and karts).

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the server config
!server_config.xml
```
