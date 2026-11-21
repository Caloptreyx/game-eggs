# CryoFall

CryoFall is a sci-fi multiplayer survival and colony game from AtomicTorch Studio: build a base, research technologies, craft, trade and fight on a hostile alien planet together with other players.

Native Calagopus egg for the CryoFall dedicated server (Steam app `1061710`), installed anonymously through SteamCMD, with the .NET runtime its runtimeconfig asks for in `.dotnet/`. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Port, server name, description and public listing are written to `Data/SettingsServer.xml` on every start; everything else in it stays yours.
- With Public on, the server registers with the CryoFall master server and gets a public GUID that players can also use to find it.
- Console commands go straight to the server, e.g. `help`, `save`, `load`; stopping the server sends `stop 10`, which saves the world and shuts down (this took between 15 seconds and a little over a minute in testing).
- Failed installations are reported to Calagopus as failed, with the reason.

The server compiles the game scripts on start with a second .NET process; both run in globalization-invariant mode (the image has no ICU library).

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 6000    | UDP      | yes (primary allocation) |

## Configuration

`Data/SettingsServer.xml` is seeded by the installer and completed (with documentation for every setting) by the server on its first start: welcome message, server operators and access lists, network and master server options. `Data/ServerRates.config` holds the gameplay rates (gathering, crafting, PvP/PvE, ...). Mods go in `Data/Mods/` and are enabled in `Data/ModsConfig.xml`. Edit the files while the server is stopped; the panel's values are written on every start.

## Saves

The world is saved to `Data/Saves/` (`Save0.save`, with older saves kept as backups) on `save`, on stop, and periodically; `loadOrNew` loads the newest save on start.

## Requirements

About 950 MB of memory with no players; plan for 3 GB. The installation takes about 900 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the world and the configuration:

```
# Ignore all
*
# Except saves, settings and mods
!Data/Saves/
!Data/*.xml
!Data/*.config
!Data/Mods/
```
