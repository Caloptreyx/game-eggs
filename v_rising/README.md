# V Rising

V Rising is an open-world vampire survival game: awaken as a weakened vampire, hunt for blood, build your castle and rise to power, alone, with friends or against other vampires in PvP.

Native Calagopus egg for the V Rising dedicated server (Steam app `1829350`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, description, password, max players, save name, game settings and difficulty presets, server list visibility, VAC, autosave and RCON are set from the panel on every start, through the `VR_*` environment variables the server reads (its official override mechanism), so `ServerHostSettings.json` does not need editing.
- The server log is streamed to the console. The server reads no console input; use RCON for remote administration.
- Stopping the server saves the world first, then shuts it down.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 9876    | UDP      | yes (primary allocation) |
| Query | 27016   | UDP      | yes, additional allocation set as Query Port (Steam server list, A2S) |
| RCON  | 25575   | TCP      | only when RCON is on (RCON Password set); additional allocation set as RCON Port |

## Configuration

The settings files live in `save-data/Settings/`: `ServerHostSettings.json` (host settings; the panel variables override it on every start) and `ServerGameSettings.json` (game rules such as loot, crafting and blood drain rates, castle limits, PvP times). The game rules come from the Game Settings Preset when one is set (e.g. `StandardPvP`, `StandardPvE`, `DuoPvP`, `HardcorePvP`); leave it empty to use `ServerGameSettings.json`. Edit the files while the server is stopped.

Extra Arguments takes further launch parameters, e.g. `-lowerFPSWhenEmpty true`.

## Admins

Add the SteamID64 of each admin to `save-data/Settings/adminlist.txt`, one per line. Admins then open the in-game console and run `adminauth`. Bans are kept in `save-data/Settings/banlist.txt`.

## Saves

Worlds are stored in `save-data/Saves/v4/<Save Name>/`, as rotating autosaves (`AutoSave_<n>.save.gz`, Auto Save Count of them). The server autosaves every Auto Save Interval and when it is stopped from the panel, and loads the most recent save on start. A new Save Name starts a new world.

## Requirements

About 3-3.3 GB of memory with a new world and no players; plan for 6-8 GB, more with many players and castles. The installation takes about 2.2 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and settings:

```
# Ignore all
*
# Except the V Rising worlds and settings
!save-data/
```
