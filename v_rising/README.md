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

## BepInEx

`egg-v-rising-bepinex.yml` is the same egg with the [BepInEx](https://github.com/BepInEx/BepInEx) mod loader for server-side plugins, from the [BepInExPack for V Rising](https://thunderstore.io/c/v-rising/p/BepInEx/BepInExPack_V_Rising/) on Thunderstore. Panel variables, ports, saves and stop work as above.

- The installer downloads the BepInExPack (the latest version, or the one set as BepInEx Version) and copies it over the server files. Reinstall to update it; plugins and their configs in `BepInEx/` are kept.
- The startup command makes Wine load the pack's `winhttp.dll` (Doorstop) before its own (`WINEDLLOVERRIDES=winhttp=n,b`), which starts BepInEx.
- Put plugins (`.dll`) in `BepInEx/plugins/`; their settings appear in `BepInEx/config/` after the first start. The BepInEx log is `BepInEx/LogOutput.log` (e.g. `Loading [VampireCommandFramework 0.11.0]`).
- On the first start, and after each game update, BepInEx generates its interop assemblies (`BepInEx/interop/`). This takes about a minute and **more than 8.6 GB of memory**: with an 8 GB limit the server is killed (out of memory) and restarted several times before it gets through. Give the server at least 12 GB; afterwards it uses about 3.6 GB with no players.
- BepInEx and plugins must match the game version. A V Rising update can break them until new versions are released; set Auto Update to 0 to stay on a working game version.

For backups, also keep `!BepInEx/plugins/` and `!BepInEx/config/` in the ignore file below.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and settings:

```
# Ignore all
*
# Except the V Rising worlds and settings
!save-data/
```
