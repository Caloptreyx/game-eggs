# Holdfast: Nations At War

Holdfast: Nations At War is a multiplayer shooter set in the Napoleonic era: line battles, sieges, naval combat and Frontlines offensives for up to 150 players.

Native Calagopus egg for the Holdfast dedicated server (Steam app `1424230`), installed anonymously through SteamCMD. It uses the **native Linux build** and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, region, passwords, welcome message and ports are written to `serverconfig_panel.txt` on every start; everything else in it (map rotations, intro texts, game modes) stays yours.
- The server log is shown in the console; player, chat, admin, score and VAC logs go to the `logs_*` folders.
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads no console input; admins log in in game with `rc login <admin password>` and then use `rc` commands. The panel's stop sends SIGINT and the server exits at once. Holdfast keeps no world state.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 20100   | UDP      | yes (primary allocation) |
| Query | 27000   | UDP      | yes, additional allocation set as Query Port (Steam server browser, A2S) |

## Configuration

`serverconfig_panel.txt` is created once from the shipped `serverconfig_default.txt` (army battlefield rotation on PalisadeArena and SnowyPlains). For Frontlines, copy the rotations from `serverconfig_frontlines_default.txt` (and its `frontlines_mode true` line); more rotations are in `example_map_rotations/`. Each rotation is a `!map_rotation start` ... `!map_rotation end` block with the map, game mode, round time, player limits, time of day and weather. Edit the file while the server is stopped.

Bans and mutes are kept in `bannedplayers.txt`, `muteplayerschat.txt`, `muteplayersvoip.txt` and `micspammers.txt`.

## Requirements

About 660 MB of memory with no players; plan for 4 GB for full servers. The installation takes about 10 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and ban lists:

```
# Ignore all
*
# Except the server config and ban/mute lists
!serverconfig_panel.txt
!*.txt
steam_appid.txt
```
