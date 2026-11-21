# Half-Life Dedicated Server

The Half-Life Dedicated Server (HLDS) runs the multiplayer games of the GoldSource engine: Half-Life Deathmatch, Counter-Strike, Condition Zero, Day of Defeat, Team Fortress Classic, Opposing Force, Deathmatch Classic and Ricochet, and mods built on them.

Native Calagopus egg for HLDS (Steam app `90`), installed anonymously through SteamCMD with the game chosen in Game. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image. For a Counter-Strike 1.6 server with Counter-Strike defaults, the Counter-Strike 1.6 egg is the simpler choice.

## Capabilities

- Game: `valve` (Half-Life Deathmatch), `cstrike`, `czero`, `dod`, `tfc`, `gearbox` (Opposing Force), `dmc` or `ricochet`; reinstall after changing it.
- Server update through SteamCMD on every start (Auto Update).
- Server name, password and RCON password are written to `<game>/panel.cfg` and executed on every start; start map, player limit and further launch parameters (Extra Arguments) come from the panel.
- Console commands go straight to the server, e.g. `status`, `changelevel <map>`, `kick <name>`, `say <text>`. Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

GoldSource games keep no world state.

## Installation notes

App 90 only installs a game's files completely after a first SteamCMD pass with the Windows platform; the installer does that pass, then the Linux pass, and removes the Windows binaries afterwards. Lines setting `hostname`, `sv_password` or `rcon_password` in the game's `server.cfg` are commented out, since they would override the panel.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 27015   | UDP      | yes (primary allocation; also answers Steam server queries) |

## Configuration

| File | Contents |
|------|----------|
| `<game>/server.cfg` | Server settings run on every map |
| `<game>/mapcycle.txt` | Map rotation |
| `<game>/motd.txt` | Message of the day |
| `<game>/banned.cfg`, `<game>/listip.cfg` | Banned SteamIDs and IP addresses |

Mods and plugins (Metamod, AMX Mod X, ...) go into the game folder (not installed by the egg).

## ReHLDS

`egg-hlds-rehlds.yml` runs the same games on [ReHLDS](https://github.com/rehlds/ReHLDS), the reverse-engineered GoldSource engine, installed from its newest upstream release. The released ReHLDS only runs on the pre-25th-anniversary engine files, so this egg installs and updates the `steam_legacy` branch of app 90. The ReHLDS engine is kept in `.rehlds/` and copied over the Steam files on every start, so an HLDS update never brings back the stock engine; reinstall to update ReHLDS. `version` in the console shows the ReHLDS build. For Counter-Strike with ReGameDLL_CS, Metamod-R, AMX Mod X and ReAPI preinstalled, use the Counter-Strike 1.6 ReHLDS egg.

## Requirements

About 85 MB of memory with no players; plan for 256 MB. The installation takes about 1.6 GB of disk (all GoldSource games share app 90's files).

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the game's configuration (Day of Defeat shown):

```
# Ignore all
*
# Except the game's configuration, map cycle and bans
!dod/*.cfg
!dod/*.txt
!dod/addons/
```
