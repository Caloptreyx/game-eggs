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

Plugins such as Metamod and AMX Mod X can be added under `cstrike/addons/` (the vanilla egg does not install them; the ReHLDS egg below does).

## ReHLDS

`egg-counter-strike-1-6-rehlds.yml` runs the same server on [ReHLDS](https://github.com/rehlds/ReHLDS), the reverse-engineered GoldSource engine, with ReGameDLL_CS, Metamod-R, AMX Mod X 1.10 and ReAPI, all from their upstream releases at install time.

- The released ReHLDS only runs on the pre-25th-anniversary engine files (the current HLDS `steam_api` lacks `SteamGameServer_Init`), so this egg installs and updates the `steam_legacy` branch of app 90.
- The ReHLDS engine and the ReGameDLL `cs.so` are kept in `.rehlds/` and copied over the Steam files on every start, and `cstrike/liblist.gam` is pointed at Metamod-R, so an HLDS update never brings back the stock engine.
- Reinstalling updates all components; existing `config.ini`, `plugins.ini`, AMX Mod X configs and ReGameDLL's `game.cfg`/`game_init.cfg` are kept.
- `version`, `game version`, `meta list` and `amxx modules` in the console show the running versions.

It needs about 75 MB of memory with no players and about 1.2 GB of disk.

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
