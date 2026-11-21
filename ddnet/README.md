# DDNet

DDraceNetwork (DDNet) is a free cooperative 2D platformer based on Teeworlds: players race through maps together using hook, jump, hammer and teamwork, with thousands of community maps.

Native Calagopus egg for the official DDNet server (Linux build from ddnet.org). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; no Steam account is involved.

## Capabilities

- Installs the current release or a fixed version (DDNet Version). Reinstall to update; settings, maps and ranks in `ddnet-data/` are kept.
- Server name, passwords, start map, player limit and server list registration are set from the panel on every start; any other console command through Extra Arguments.
- Ranks and times are kept in the server's SQLite database.
- The panel's stop sends SIGINT; the server unregisters from the server list and exits cleanly.
- Failed installations are reported to Calagopus as failed, with the reason.

The console shows the server log. The server reads no console input; admins log in to the in-game remote console (F2, `rcon_auth`/password prompt) with the RCON Password.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 8303    | UDP      | yes (primary allocation) |

With Public on, the server registers with the DDNet master servers over IPv4 (the container has no IPv6).

## Configuration

The server's own folder is `ddnet-data/ddnet/`. Its `autoexec_server.cfg` (created by the installer) runs on every start, before the panel settings, which therefore win. Add any other setting there, one console command per line; see the [settings and commands list](https://ddnet.org/settingscommands/). Extra Arguments takes further commands separated by `;`, e.g. `sv_vote_kick 0; sv_test_cmds 0`.

Custom maps go in `ddnet-data/ddnet/maps/` (the built-in maps are in `data/maps/`); set Map to the file name without `.map`.

## Saves

`ddnet-data/ddnet/` holds the ranks database (`ddnet-server.sqlite`), the server log (`ddnet-server.log`), demos, ghosts and teehistorian records.

## Requirements

About 40 MB of memory with no players; plan for 256 MB. The installation takes about 80 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the server folder:

```
# Ignore all
*
# Except the DDNet settings, maps and ranks
!ddnet-data/
```
