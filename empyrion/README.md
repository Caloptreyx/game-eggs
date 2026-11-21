# Empyrion: Galactic Survival

Empyrion: Galactic Survival is an open-world space survival sandbox. Players build ships and bases, explore and fight on planets and in space, and trade across a galaxy, alone or together.

Calagopus egg for the Empyrion dedicated server (Steam app `530870`), installed anonymously through SteamCMD. The server only exists for Windows; it runs through Proton (with Xvfb) in the `ghcr.io/caloptreyx/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Port, name, description, visibility, password, player limit, game name, mode, seed, scenario and EasyAntiCheat are written to `dedicated.yaml` on every start. The shipped file has password, player limit, description and the telnet console only as comments, so the installer adds them once.
- The server log is streamed from `Logs/server/server.log`. Lines typed in the panel console go to the server's telnet console (`help` lists the commands), and its answers are printed.
- The panel's stop saves the game and stops the server.
- Failed installations are reported to Calagopus as failed, with the reason.

A signal kills the server without saving. The startup command therefore runs it in its own session and catches the panel's stop signal: it sends `saveandexit 0` to the local telnet console (through bash's `/dev/tcp`), and the server saves and exits. The telnet console only listens on localhost.

Save games are in `Saves/Games/<Game Name>/`; a new Game Name starts a new game with the current mode, seed and scenario. Playfields run in separate playfield server processes that the server starts as needed.

## Server Ports

Empyrion uses the game port and the ports after it, so give the server consecutive allocations with the lowest as the primary:

| Port      | Default | Protocol | Notes |
|-----------|---------|----------|-------|
| Game      | 30000   | UDP/TCP  | primary allocation |
| Query     | 30001   | UDP      | game port + 1; Steam server browser (A2S) |
| +2, +3    | 30002-30003 | UDP/TCP | the server also binds these; +3 is the local playfield server port |

## Requirements

About 2.1 GB of memory for the server with no players, plus about 1 GB per running playfield server; plan for 8 GB or more. The installation takes about 19 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the save games and settings:

```
# Ignore all
*
# Except the saves and the server config
!Saves/
!dedicated.yaml
```
