# Multi Theft Auto

Multi Theft Auto: San Andreas (MTA:SA) is the multiplayer modification for Grand Theft Auto: San Andreas, with a Lua scripting engine for any game mode: race, freeroam, roleplay, deathmatch and more.

Native Calagopus egg for the official MTA:SA Linux server (from linux.multitheftauto.com) with the default resource pack. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved. Players need their own copy of GTA: San Andreas and the MTA client; the server needs neither.

## Capabilities

- Installs the current MTA:SA server, the base config and the default resources (race, freeroam, play, admin, ...); reinstall to update the server, the config and resources are kept.
- Server name, password, player limit, ports, public listing and owner email are written to `mods/deathmatch/mtaserver.conf` on every start; everything else in it stays yours.
- Console commands go straight to the server, e.g. `help`, `start <resource>`, `stop <resource>`, `restart <resource>`, `refresh`, `kick`, `ban`, `say <text>`. Stopping the server sends `shutdown`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 22003   | UDP      | yes (primary allocation) |
| HTTP  | same as Game | TCP | yes: the server's HTTP server (resource downloads for players) shares the game port number over TCP |
| ASE   | Game + 123 | UDP   | for the in-game server browser (Public on); add it as an additional allocation |

The server registers with the MTA master server on start ("Querying MTA master server... success!").

## Configuration

`mods/deathmatch/` holds the configuration: `mtaserver.conf` (server settings, resources started on boot), `acl.xml` (access control: admin groups and rights) and the resources in `resources/`. Edit them while the server is stopped, or use the console and the admin resource.

## Admins

Create an account in game (`/register <name> <password>`), then add it to the Admin group in `acl.xml` (`<object name="user.<name>"></object>`) while the server is stopped, or from the console with `aclrequest` and the admin resource. The admin group is protected by authorized serials (see the console notice).

## Saves

Server data (accounts in `internal.db`, the registry, ban list, logs and automatic backups in `backups/`) is kept in `mods/deathmatch/`.

## Requirements

About 90 MB of memory with the default resources and no players; plan for 512 MB-1 GB. The installation takes about 77 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the server data:

```
# Ignore all
*
# Except the MTA:SA config, resources and databases
!mods/deathmatch/
```
