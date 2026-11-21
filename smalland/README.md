# Smalland: Survive the Wilds

Smalland: Survive the Wilds is a multiplayer survival adventure in which the players are tiny in a giant world of insects and creatures. They craft, build, tame creatures and survive together.

Native Calagopus egg for the Smalland dedicated server (Steam app `808040`), installed anonymously through SteamCMD. The server is listed through Epic Online Services (EOS) with the game's public dedicated server credentials, which the startup command passes.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, world name, password, visibility and the gameplay settings (friendly fire, peaceful mode, keep inventory, deterioration, day and season length, creature health and damage, nourishment loss, fall damage) are map URL options built from the panel variables on every start.
- The Unreal log is shown in the console. The server reads no console input.
- The panel's stop sends SIGINT; the server saves the world and exits.
- Failed installations are reported to Calagopus as failed, with the reason.

Worlds are saved as `SMALLAND/Saved/SaveGames/Worlds/<World Name>.wld`; a new World Name creates a new world. The server has 12 player slots.

## Server Ports

| Port | Default | Protocol |
|------|---------|----------|
| Game | 7777    | UDP      |

## Requirements

About 3.5 GB of memory with no players; plan for 6 GB. The installation takes about 2 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds:

```
# Ignore all
*
# Except the worlds
!SMALLAND/Saved/SaveGames/
```
