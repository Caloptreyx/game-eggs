# Counter-Strike 1.6

Counter-Strike 1.6 is the classic team-based tactical shooter on the GoldSource engine: terrorists against counter-terrorists in bomb defusal and hostage rescue rounds on maps such as de_dust2, de_inferno and cs_office.

Native Calagopus egg for the Half-Life Dedicated Server (HLDS, Steam app `90`) with the Counter-Strike mod, installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, password and RCON password are written to `cstrike/server.cfg` on every start; everything else in it stays yours.
- Start map, player limit and further launch parameters (Extra Arguments) from the panel.
- Console commands go straight to the server, e.g. `status`, `changelevel <map>`, `kick <name>`, `say <text>`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

Counter-Strike keeps no world state.

## Installation notes

App 90 only installs the complete Counter-Strike files after a first SteamCMD pass with the Windows platform; the installer does that pass, then the Linux pass, and removes the Windows binaries afterwards.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP      | yes (primary allocation; also answers Steam server queries) |

## Configuration

| File | Contents |
|------|----------|
| `cstrike/server.cfg` | Server settings run on every map: round, buy and freeze times, money, team balance, logging |
| `cstrike/mapcycle.txt` | Map rotation |
| `cstrike/motd.txt` | Message of the day shown on join |
| `cstrike/banned.cfg`, `cstrike/listip.cfg` | Banned SteamIDs and IP addresses |

Plugins such as Metamod and AMX Mod X can be added under `cstrike/addons/` (not installed by the egg).

## Requirements

About 80 MB of memory with no players; plan for 256 MB. The installation takes about 1.25 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Counter-Strike configuration, map cycle and bans
!cstrike/*.cfg
!cstrike/*.txt
!cstrike/addons/
```
