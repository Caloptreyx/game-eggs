# LAC - Los Angeles Crimes Online

Los Angeles Crimes (LAC) Online is a free open-world sandbox shooter. Players drive, fight and roleplay in a city, play ragdoll party, car soccer or zombie survival maps, and build their own maps with the editor.

Calagopus egg for the LAC Linux dedicated server from the official [builds page](https://lacrimesonline.com/builds/).

## Capabilities

- Installs the newest Linux build listed on the builds page, or the one set in Version; reinstall to update. The installed version is kept in `.lac-version`, and old builds are removed on update.
- Player limit, port, map (or map rotation), password, admin code, language and the community list setting are written to `.config/unity3d/MA/LAC/ServerConfig.txt` on every start. The installer seeds the file with the server's defaults; lock down, restart timer, roleplay and anti-cheat settings stay yours.
- The Unity log is shown in the console, including the community list registration (`[ShareServer] ... success`). The server reads no console input; players become admin by typing `/<Admin Code>` in the chat.
- The panel's stop sends SIGTERM (the server ignores SIGINT).
- Failed installations are reported to Calagopus as failed, with the reason.

Player data (roleplay accounts, bans, skins) and editor maps are kept in `.config/unity3d/MA/LAC/`. Change Admin Code from its default `YOUR-NUM-CODE`.

## Server Ports

| Port | Default | Protocol |
|------|---------|----------|
| Game | 7777    | UDP      |

## Requirements

About 320 MB of memory with no players; plan for 1 GB. The installation takes about 260 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings and player data:

```
# Ignore all
*
# Except the settings, accounts and editor maps
!.config/
```
