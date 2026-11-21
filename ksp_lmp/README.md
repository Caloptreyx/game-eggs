# Kerbal Space Program: Luna Multiplayer

[Luna Multiplayer](https://github.com/LunaMultiplayer/LunaMultiplayer) (LMP) is a multiplayer mod for Kerbal Space Program: players share one universe, fly and dock together and build bases, with warp handled through subspaces.

Native Calagopus egg for the LMP server: the official self-contained Linux build (`LunaMultiplayer-Server-linux-x64-Release.zip`) from the LMP GitHub releases, with the ICU libraries it needs in `.icu/`. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image. Players need Kerbal Space Program with the matching LMP client mod.

## Capabilities

- Install of the latest release or a fixed tag (LMP Version); reinstall to update. `LMPServer/Config`, `LMPServer/Universe`, plugins and logs are kept.
- Server name, description, passwords, player limit, game mode, port, master server registration and the status website are written to the XML settings on every start; the other settings stay yours.
- Console commands go straight to the server: `/help`, `/listclients`, `/kick`, `/ban`, `/say`, `/backup now`, `/dekessler`, `/nukeksc`, ...
- The panel's stop sends SIGINT: the server backs the universe up and shuts down.
- Optional status website: the server status, settings and players as JSON over HTTP.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port    | Default | Protocol | Required |
|---------|---------|----------|----------|
| Game    | 8800    | UDP      | yes (primary allocation) |
| Website | 8900    | TCP      | only with Website on: an additional allocation set as Website Port |

## Configuration

The settings are XML files in `LMPServer/Config/`: `GeneralSettings.xml` (name, passwords, game mode and difficulty, player limit, cheats, asteroids), `GameplaySettings.xml` (career/science balance), `WarpSettings.xml` (subspace or master-controlled warp), `IntervalSettings.xml` (backup interval), `LMPModControl.xml` (allowed mods and parts) and more. Edit them while the server is stopped.

The server rewrites these files on every start and declares them as UTF-16 although they are UTF-8; the egg corrects the declaration after the server stops so the panel can keep editing them. Game Mode applies to a new universe (delete `LMPServer/Universe` to start over).

## Saves

The universe (vessels, kerbals, scenarios) lives in `LMPServer/Universe`. The server backs it up periodically (`IntervalSettings.xml`), on `/backup now` and on stop.

## Requirements

About 140 MB of memory with no players; plan for 1 GB. The installation takes about 70 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings and the universe:

```
# Ignore all
*
# Except the settings, universe and plugins
!LMPServer/Config/
!LMPServer/Universe/
!LMPServer/Plugins/
```
