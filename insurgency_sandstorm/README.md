# Insurgency: Sandstorm

Insurgency: Sandstorm is a team-based tactical first-person shooter of close-quarters combat and objective-oriented multiplayer, in versus (PvP) and co-op (PvE) modes.

Native Calagopus egg for the Insurgency: Sandstorm dedicated server (Steam app `581330`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, start map and scenario, max players and password are set from the panel on every start.
- Optional Game Server Login Token (online listing) and Game Stats token (experience for players on the server).
- Failed installations are reported to Calagopus as failed, with the reason.

The console shows the server log; the server reads no console input. Admins listed in `Admins.txt` use the in-game admin menu and console.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 27102   | UDP      | yes (primary allocation) |
| Query | 27131   | UDP      | yes, additional allocation set as Query Port (Steam server queries, A2S) |

## Game Server Login Token

Without a token the server is not listed in the online server browser. Create one at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with app ID `581320` and enter it in Game Server Login Token. A Game Stats token (from gamestats.sandstorm.game) lets players earn experience on the server.

## Configuration

| Path | Contents |
|------|----------|
| `Insurgency/Saved/Config/LinuxServer/Game.ini` | Game mode and server settings (created empty by the installer) |
| `Insurgency/Saved/Config/LinuxServer/Engine.ini` | Engine settings |
| `Insurgency/Config/Server/Admins.txt` | Admins, one SteamID64 per line |
| `Insurgency/Config/Server/MapCycle.txt` | Map cycle, one scenario per line (e.g. `Scenario_Crossing_Checkpoint_Security`) |

Map and Scenario must match (e.g. `Canyon` with `Scenario_Crossing_Checkpoint_Security`). Extra Arguments takes further options such as `-ruleset=CompetitiveFirefight` or `-Mods`.

## Saves

Insurgency: Sandstorm keeps no world state: every match starts fresh. Only the config files above are worth backing up. The panel's stop sends SIGINT, which shuts the server down cleanly within seconds.

## Requirements

About 1.4 GB of memory on Canyon (Checkpoint) with no players; plan for 4 GB, more with a full server. The installation takes about 5.1 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Insurgency: Sandstorm configuration
!Insurgency/Saved/Config/
!Insurgency/Config/Server/
```
