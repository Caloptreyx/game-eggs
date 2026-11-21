# Mordhau

Mordhau is a medieval first-person slasher with a skill-based melee combat system: large frontline battles, duels, Battle Royale, Horde and Skirmish for up to 64 players.

Native Calagopus egg for the Mordhau dedicated server (Steam app `629800`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- Server name, password, admin password and player slots are written to `Game.ini` on every start; the map rotation and everything else in that file stay yours.
- Start map from the panel, or the first map of the map rotation.
- Failed installations are reported to Calagopus as failed, with the reason.

The console shows the server log. The server reads no console input; admins use the in-game console (`AdminLogin <Admin Password>`, then `ChangeLevel`, `Kick`, `Ban`, ...).

## Server Ports

| Port   | Default | Protocol | Required |
|--------|---------|----------|----------|
| Game   | 7777    | UDP      | yes (primary allocation) |
| Query  | 27015   | UDP      | additional allocation set as Query Port; the current build did not open it in our test |
| Beacon | 15000   | UDP      | yes, additional allocation set as Beacon Port |

The server registers itself with PlayFab, which is how players find it in the server browser; in our test it did not answer Steam A2S queries.

## Configuration

`Mordhau/Saved/Config/LinuxServer/Game.ini` is created by the installer with the default map rotation. The panel writes Server Name, Server Password, Admin Password and Max Players (`MaxSlots`) into `[/Script/Mordhau.MordhauGameSession]` on every start. Edit the rest while the server is stopped:

- `[/Script/Mordhau.MordhauGameMode]`: `MapRotation=` lines (one map per line, e.g. `FFA_ThePit`, `SKM_Grad`, `FL_Taiga`), respawn times, damage factors.
- `[/Script/Mordhau.MordhauGameSession]`: `Admins=` (SteamID64 per line), `BannedPlayers`, `RconPassword` / `RconPort` for remote RCON tools.

Leave Map empty to start on the first map of the rotation.

## Saves

Mordhau keeps no world state: every match starts fresh. Only `Mordhau/Saved/Config/LinuxServer/` is worth backing up. The panel's stop sends SIGINT, which shuts the server down cleanly within seconds.

## Requirements

About 0.6 GB of memory on FFA_ThePit with no players; plan for 2-4 GB, more for large Frontline maps and full servers. The installation takes about 5.6 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration:

```
# Ignore all
*
# Except the Mordhau server configuration
!Mordhau/Saved/Config/LinuxServer/
```
