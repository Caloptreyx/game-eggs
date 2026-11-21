# Renown

Renown is an intense medieval multiplayer survival game in a vast, ever-evolving world: gather resources, build strongholds and fight brutal physics-based melee battles, alone or with your clan.

Native Calagopus egg for the Renown dedicated server (Steam app `1512690`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from the `beta` or `experimental` branch.
- Server name, passwords, max players, region, message of the day, save name and RCON are written to `Game.ini` on every start; other settings in that file are kept.
- Console input: lines typed in the panel console are sent to the server over RCON, e.g. `help`, `playerlist`, `saveworld`, `broadcast <message>`, `kickplayer <id>`, `banplayer <id>`, `addadmin <SteamID>`, `listmaps`, `changemap <map>`.
- Stopping the server saves the world first (`saveworld`), then shuts it down.
- Failed installations are reported to Calagopus as failed, with the reason.

Use the panel's stop button, not the RCON command `DoExit`: the panel treats a server that exits on its own as crashed and starts it again.

## Server Ports

| Port   | Default | Protocol | Required |
|--------|---------|----------|----------|
| Game   | 7777    | UDP      | yes (primary allocation) |
| Query  | 27015   | UDP      | no: passed to the server as `-QUERYPORT`, but not opened by the current build |
| Beacon | 9999    | UDP      | yes, additional allocation set as Beacon Port |
| RCON   | 25575   | TCP      | additional allocation set as RCON Port; reachable from outside only for remote RCON tools |

The server registers itself with Epic Online Services, which is how players find it in the server browser; in our test it did not open the Query port or answer Steam queries. The EOS client id and secret it needs are the public values Renown's own launcher passes to every dedicated server; the egg passes them on the command line.

## Configuration

The settings are in `Renown_Combat/Saved/Config/LinuxServer/Game.ini`. The installer creates it with the keys the panel manages; on every start the panel writes Server Name, Server Password, Admin Password, Max Players (in both places Renown reads it), Region, Message of the Day, Save Name and RCON (always on, with the RCON Port and Password, since the console uses it).

Other settings, such as the map rotation (`MapRotation=RSurv-AlgaraIsle_p`, one line per map; the map prefix sets the game mode), the wipe schedule and the survival multipliers, are added to the same `[/Game/Core/Blueprints/Renown_GameInstance.Renown_GameInstance_C]` section by hand while the server is stopped; see the [Renown INI files wiki page](https://renown.fandom.com/wiki/INI_Files).

The RCON Password is required (at least 6 characters, no spaces). Note that the server prints the RCON password of every RCON login in its log.

## Admins

Add admins from the console with `addadmin <SteamID64>`; `getadminwhitelist` lists them. The whitelist is stored with the saves. Players can also use the Admin Password in game.

## Saves

Survival saves are stored in `Renown_Combat/Saved/SaveGames/` as `<Save Name>.sav`, with timestamped backups next to them, plus the admin whitelist, player tokens and faction saves. The server autosaves every 5 minutes, on `saveworld` and when stopped from the panel. A new Save Name starts a new world; the old one is kept. Survival worlds are wiped on Renown's wipe schedule (see the Configuration).

## Requirements

About 5.5-6 GB of memory on Algara Isle with no players; plan for 8 GB, more with many players. The installation takes about 10 GB of disk; a new survival save about 110 KB per copy.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the saves and settings:

```
# Ignore all
*
# Except the Renown saves and settings
!Renown_Combat/Saved/SaveGames/
!Renown_Combat/Saved/Config/
```
