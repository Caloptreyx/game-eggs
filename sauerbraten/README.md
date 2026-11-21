# Cube 2: Sauerbraten

Cube 2: Sauerbraten is a free and open source arena shooter in the spirit of Quake: fast deathmatch, instagib, capture and CTF modes, plus cooperative in-game map editing.

Native Calagopus egg for the official Sauerbraten Linux server (`bin_unix/linux_64_server`, 2020 Edition from SourceForge). Only the server, its documented `server-init.cfg` and the docs are installed; maps and media are not needed on a server. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server name, player limit, server and admin passwords and master server registration are written to `server-init.cfg` on every start; everything else in it stays yours.
- The server registers with the master server (`master.sauerbraten.org`) when Public is on.
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads no console input; admins claim rights in game with `/setmaster <admin password>`. The panel's stop sends SIGINT and the server exits at once. Sauerbraten keeps no world state.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 28785   | UDP      | yes (primary allocation) |
| Info | 28786   | UDP      | yes: always the game port + 1 (server browser pings); allocate it too |

## Configuration

`server-init.cfg` in the server root holds the server settings: bot limit, master mode (`publicserver`), message of the day (`servermotd`), and more; see `docs/config.html`. The panel's values are written into it on every start, so edit the other lines while the server is stopped.

## Requirements

A few MB of memory with no players; plan for 128 MB. The installation takes under 2 MB of disk, but the installer downloads the 1 GB release archive into the server folder while it runs, so the server needs about 1.1 GB of disk to install.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the server config
!server-init.cfg
```
