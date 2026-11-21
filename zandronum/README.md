# Zandronum

Zandronum is a multiplayer Doom source port descended from Skulltag: deathmatch, cooperative, capture the flag, invasion, last man standing and more, with mods and custom WADs.

Native Calagopus egg for the official Zandronum Linux server (`zandronum-server`, from zandronum.com) with the free [Freedoom](https://freedoom.github.io/) IWADs (`freedoom1.wad`, `freedoom2.wad`, `freedm.wad`). It runs on the `ghcr.io/caloptreyx/games:zandronum` image, which adds the SDL 1.2 and libjpeg 8 libraries the official build is linked against.

## Capabilities

- Install of the newest Linux build or a fixed version (Zandronum Version); reinstall to update. `server.cfg`, `zandronum.ini`, your WADs and the Freedoom IWADs are kept.
- The installer creates `server.cfg` once with the game mode (deathmatch), limits and a map rotation (MAP01-MAP08).
- Server name, passwords, player limit, master server listing, IWAD, extra WADs and the start map are set from the panel on every start; further parameters through Extra Arguments.
- Console commands go straight to the server, e.g. `map MAP05`, `kick <name>`, `say <text>`, `addmap <map>`, or any cvar (`sv_hostname`). Stopping the server sends `quit`.
- Failed installations are reported to Calagopus as failed, with the reason.

Zandronum keeps no world state.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 10666   | UDP      | yes (primary allocation; also answers launcher queries such as Doomseeker) |

## WADs

- **IWAD**: the base game in the server root. `freedm.wad` (deathmatch maps, the default), `freedoom2.wad` (Doom II replacement, MAP01-MAP32) or `freedoom1.wad` (Doom replacement, E1M1-E4M9). Commercial IWADs (`doom2.wad`, `doom.wad`, `plutonia.wad`, `tnt.wad`) work when you upload them to the server root; players need the same IWAD.
- **PWADs**: mods and map packs go in `wads/`; list their file names, separated by spaces, in PWADs.

Match the start map and the rotation in `server.cfg` to the IWAD (MAP01 style or E1M1 style).

## Configuration

`server.cfg` runs first on every start; the panel settings are passed afterwards and win. Edit it for the game mode (`deathmatch`, `teamplay`, `ctf`, `cooperative`, `survival`, `invasion`, ...), limits (`fraglimit`, `timelimit`), the message of the day (`sv_motd`), and the map rotation (`addmap`). `zandronum.ini` keeps the server's saved cvars.

## Requirements

About 21 MB of memory with no players; plan for 256 MB. The installation takes about 90 MB of disk (most of it the Freedoom IWADs).

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the configuration and your WADs:

```
# Ignore all
*
# Except the configuration and added WADs
!server.cfg
!zandronum.ini
!wads/
```
