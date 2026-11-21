# Nuclear Option

Nuclear Option is a near-future air combat game with realistic flight physics: fly jets and helicopters against land, air and sea threats, in PvE or PvP, up to tactical and strategic nuclear weapons.

Native Calagopus egg for the Nuclear Option dedicated server (Steam app `3930080`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- Server name, password, player limit, server browser visibility, ports and the mission folder are written to `DedicatedServerConfig.json` on every start; the mission rotation and other settings stay yours.
- The server log is shown in the console. The server reads no console input; the panel's stop sends SIGINT.
- Failed installations are reported to Calagopus as failed, with the reason.

Matches follow the mission rotation; the server keeps no world state between missions.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation; written to the config as Port) |
| Query | 27015   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |

Players connect through Steam networking (the server logs "Setting Steam socket"); the query port answers the server browser.

## Configuration

`DedicatedServerConfig.json` in the server root holds the mission rotation (`MissionRotation`, `RotationType`; built-in missions such as Escalation and Terminal Control, or custom missions placed in `NuclearOption-Missions/`), `ModdedServer`, ban list paths (`ban_list.txt`), vote-kick settings, and the idle timers (`NoPlayerStopTime`, `PostMissionDelay`). Edit it while the server is stopped.

## Requirements

About 380 MB of memory with no players; plan for 2 GB. The installation takes about 1.3 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and missions:

```
# Ignore all
*
# Except the configuration, ban list and custom missions
!DedicatedServerConfig.json
!ban_list.txt
!NuclearOption-Missions/
```
