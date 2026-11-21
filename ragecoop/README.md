# RAGECOOP

[RAGECOOP](https://github.com/RAGECOOP/RAGECOOP-V) is a cooperative multiplayer mod for Grand Theft Auto V's single player: players explore the story world together, with synced traffic, NPCs, weather and time.

Native Calagopus egg for the RAGECOOP server: the self-contained Linux build (`RageCoop.Server-linux-x64.zip`) from the RAGECOOP GitHub releases. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image. Players need GTA V with the RAGECOOP client mod of a compatible version.

## Capabilities

- Install of the latest release or a fixed tag (RAGECOOP Version); reinstall to update. `Settings.xml` and `Resources/` are kept.
- Server name, description, welcome message, player limit, port and master server listing are written to `Settings.xml` on every start; the other settings stay yours.
- With Public on, the server announces itself to the RAGECOOP master server (`masterserver.ragecoop.com`) and shows up in the in-game server list.
- The server takes console input ("Type here to send chat messages or execute commands"). The panel's stop sends SIGINT, the server's documented way to stop.
- Failed installations are reported to Calagopus as failed, with the reason.

RAGECOOP keeps no world state.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 4499    | UDP      | yes (primary allocation) |

## Configuration

`Settings.xml` holds the server settings: latency limit, game mode and language shown in the list, streaming distances, P2P, ZeroTier and voice options, spam and god-mode kicks. Server-side scripts (resources) go into `Resources/`; see the RAGECOOP repository for the resource API.

## Requirements

About 40 MB of memory with no players; plan for 512 MB. The installation takes about 65 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings and resources:

```
# Ignore all
*
# Except the settings and resources
!Settings.xml
!Resources/
```
