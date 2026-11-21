# Sonic Robo Blast 2

[Sonic Robo Blast 2](https://www.srb2.org/) is a free 3D Sonic the Hedgehog fangame built on a modified Doom engine, with co-op, race, match, tag, hide and seek and capture the flag netgames and a large add-on scene.

Native Calagopus egg for an SRB2 dedicated server: the official Windows build (`SRB2-v*-Full.zip` from the [SRB2 GitHub releases](https://github.com/STJr/SRB2/releases)) run with `-dedicated` through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image. SRB2 has no Linux release build.

## Capabilities

- Install of the latest release or a fixed version (SRB2 Version); reinstall to update. `adedserv.cfg` and added add-ons are kept.
- Server name, admin password and player limit are written to `adedserv.cfg` on every start; the server runs it when it starts.
- The server log (`latest-log.txt`) is streamed to the console. The server reads no console input; admins use the in-game console (`login <password>`).
- The panel's stop sends SIGINT, which the shell forwards to the server.
- Failed installations are reported to Calagopus as failed, with the reason.

SRB2 netgames keep no world state.

## Master server listing

Master Server Room lists the server on the SRB2 master server (33 = Standard, 28 = Casual). Under Proton the Windows build's curl currently fails to reach the master server (TLS error), so the default is 0 (unlisted); players join with the server's address.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 5029    | UDP      | yes (primary allocation) |

## Configuration

`adedserv.cfg` is a console script run when the dedicated server starts: add console variables there (game type, `pointlimit`, `timelimit`, `allowjoin`, ...). Add-ons (`.pk3`, `.wad`, `.lua`) are loaded with `-file <add-on>` in Extra Arguments; players download them when they join.

## Requirements

About 200 MB of memory with no players; plan for 512 MB. The installation takes about 200 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the server config
!adedserv.cfg
```
