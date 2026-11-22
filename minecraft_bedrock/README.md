# Minecraft: Bedrock Edition

The official Minecraft Bedrock Dedicated Server from Mojang, for players on Windows, Xbox, PlayStation, Switch, iOS and Android.

Native Calagopus egg. It runs on the `ghcr.io/caloptreyx/games:bedrock` image, which updates the server on every start, so it keeps up with Bedrock clients (they update automatically, and can only join a server on the same version).

## Capabilities

- Server update on every start (Auto Update) to `latest`, `preview`, or a fixed version such as `1.21.50.10`. Nothing is downloaded when the installed version is current.
- Server name, game mode, difficulty, max players, world, seed, cheats and allow list are set from the panel on every start.
- `server.properties`, `permissions.json`, `allowlist.json` and worlds are kept on updates and reinstalls.
- Console commands go straight to the server, e.g. `list`, `op <gamertag>`, `allowlist add <gamertag>`, `save hold`.
- Stopping the server sends `stop`, which saves the world.
- Failed installations (for example a version that does not exist) are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 19132   | UDP      | yes (primary allocation) |

The panel writes the allocation's port into `server.properties` on every start. Players on consoles can only join servers on custom ports through a featured-server workaround or a friend-invite tool; on PC and mobile, add the server by IP and port.

## Configuration

Settings not available as panel variables stay editable in `server.properties`, e.g. `view-distance`, `tick-distance`, `online-mode`, `default-player-permission-level`. The panel-managed ones are rewritten on every start, so change those in the panel.

## Worlds

Worlds are stored in `worlds/<World Name>/`. A different World Name creates a new world; the old one is kept. To use an existing world, upload its folder to `worlds/` and set World Name to the folder name.

## Requirements

About 1-2 GB of memory for a few players; more for large view or tick distances.

## PocketMine-MP

`egg-pocketmine-mp.yml` runs [PocketMine-MP](https://pmmp.io/), a Bedrock server written in PHP with its own plugin API (plugins in `plugins/`). The installer takes `PocketMine-MP.phar` from the GitHub release (latest, or the version set) and the prebuilt PHP binary that release asks for (`pm5-php-<version>-latest` from pmmp/PHP-Binaries) into `bin/`.

- One UDP port (default 19132), the primary allocation; the panel writes it, the MOTD, player limit, game mode, difficulty and whitelist into `server.properties` on every start (the installer creates the file).
- Each PocketMine-MP release supports specific Bedrock client versions (5.44.3: 1.26.30); reinstall to update when clients update.
- Console lines are server commands (`version`, `op`, `whitelist`, ...); stop sends `stop`, which saves the worlds.
- The installation takes about 45 MB of disk.
