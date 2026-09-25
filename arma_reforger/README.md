# Arma Reforger

The military sandbox on the Enfusion engine.

Native Calagopus egg for the Linux dedicated server (Steam app `1874900`), installed anonymously through SteamCMD. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- Name, passwords, scenario, players, visibility, cross-play, BattlEye, third person, and the query and RCON ports are written to `config.json` on every start.
- The console sends commands to the server over RCON once it is up, e.g. `#players`, `#kick <id>`, `#restart`.
- Workshop mods listed in `config.json` are downloaded by the server into `addons-workshop/`.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game  | 2001  | UDP | yes (primary allocation) |
| Query | 17777 | UDP | yes, for the server browser |
| RCON  | 19999 | UDP | only for remote admin tools; the panel console uses it locally |

## Mods

Add mods to the `game.mods` list in `config.json` while the server is stopped:

```json
"mods": [
    { "modId": "59727DAE364DEADB", "name": "Example Mod", "version": "" }
]
```

An empty `version` always uses the latest. With cross-play on, only console-compatible mods work.

## Scenarios

The default is Conflict - Everon, `{ECC61978EDCC2B5A}Missions/23_Campaign.conf`. Start the server once with `-listScenarios` in Extra Arguments to print every installed scenario ID, then set the one you want in Scenario.

## Configuration

Settings that are not panel variables (view distances, admins, `operating` options) stay editable in `config.json`. The server checks the file against its schema on start and refuses invalid values, printing the reason in the console.

## Requirements

About 8 GB of memory and 15 GB of disk, more with mods.
