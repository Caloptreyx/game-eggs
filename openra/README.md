# OpenRA

OpenRA is an open source engine that recreates and modernizes the classic Command & Conquer real-time strategy games: Red Alert, Tiberian Dawn and Dune 2000, with online multiplayer, new balance and quality-of-life features.

Native Calagopus egg for an OpenRA dedicated server. One egg covers all three games: the installer extracts the official AppImage of the chosen mod ([GitHub releases](https://github.com/OpenRA/OpenRA/releases)) into `openra/`, which brings its own .NET runtime, and the ICU libraries its translations need into `.icu/`. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Mod: `ra` (Red Alert), `cnc` (Tiberian Dawn) or `d2k` (Dune 2000); reinstall after changing it. `support/` (settings, maps, logs) is kept.
- Install of the current release or a fixed tag (OpenRA Version); reinstall to update. Players need the same version.
- Server name, password, port, master server listing and forum-account authentication are passed on every start; further `Server.*` settings through Extra Arguments.
- The server registers with the OpenRA master server when Public is on (password-protected servers are marked as such).
- Failed installations are reported to Calagopus as failed, with the reason.

The server reads no console input; its log is in `support/Logs/dedicated-server.log`. The panel's stop sends SIGINT and the server exits at once. Lobby settings (map, options) are chosen in game by the first player, who becomes the host; OpenRA keeps no world state.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 1234    | TCP      | yes (primary allocation) |

## Configuration

`support/settings.yaml` (created when you change settings) and the launch arguments use OpenRA's settings names, e.g. `Server.Map=<map uid>` for the initial map, `Server.EnableSingleplayer=True`, `Server.ShareAnonymizedIPs=False`, `Server.RecordReplays=True`. Custom maps go in `support/maps/<mod>/`.

## Requirements

About 110 MB of memory with no players; plan for 512 MB. The installation takes about 145 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings and maps:

```
# Ignore all
*
# Except settings and custom maps
!support/
support/Logs/
```
