# Horizon

[Horizon](https://playhorizon.dev/docs/hosting)'s server for Minecraft: Java Edition 26.3. `horizon-server` runs Horizon's own engine, a native program, instead of Mojang's Java server, with vanilla's settings, world format and console commands. Vanilla 26.3 clients and Horizon clients can join.

Native Calagopus egg. The installer downloads `horizon-server` from Horizon's GitHub releases and checks it against the SHA-256 GitHub publishes. It runs on the `ghcr.io/caloptreyx/java:25` image; the server itself does not use Java.

## Game files

Horizon reads Minecraft's data from game files prepared from Minecraft 26.3. By default (Prepare Game Files `1`) the installer prepares them: it downloads Mojang's 26.3 version file, client, client libraries, the non-sound assets and server jar, each checked against Mojang's checksum, plus a temporary Eclipse Temurin 25 runtime, and runs `horizon-prepare`. The downloads are removed afterwards; only the prepared files are kept, in `horizon/.local/horizon-client/26.3`. This takes about a minute.

Horizon itself never downloads Mojang's files: it expects them to come from your own copy of Minecraft. To keep it that way, set Prepare Game Files to `0`, prepare the files on your computer with the Horizon launcher or `horizon-prepare --prepare --server-jar server.jar`, and upload the prepared `26.3` folder as `resources/` in the server's root. A folder there takes precedence over the installer's.

## Capabilities

- Any Horizon release: `latest` (the current Nightly) or a tag from [Horizon's releases](https://github.com/horizon-rust/horizon/releases) such as `v0.2.97` or `nightly-2026-10-29`. Reinstall to update; worlds, settings and the prepared files are kept, and the game files are prepared again for the new release.
- Calagopus's EULA prompt: on first start the console asks to accept the Minecraft EULA and writes `eula.txt`.
- An existing vanilla `server.properties` in the server's root is imported into `server.toml` on the first install, so a vanilla server folder can be switched to Horizon.
- Console commands go to the game, as on vanilla. Stopping the server sends `stop`, which saves the world.
- If the game crashes, `horizon-server` restarts it by itself (after 5 s, then waiting longer, up to 5 crashes in 10 minutes).
- Failed installations are reported to Calagopus as failed, with the reason.

## Settings: `server.toml`

All settings are in `server.toml` (message of the day, max players, online mode, whitelist, view distance, game mode, difficulty, backups, and any other `server.properties` key under `[properties]`). `horizon-server` writes `server.properties` from it on every start, so edit `server.toml`, not `server.properties`. Changes apply at the next restart. The panel sets `server.port` to the allocation's port on every start.

- **Whitelist:** on by default, as `horizon-server init` sets it. Add players with `whitelist add NAME` in the console, or set `whitelist = false`.
- **Online mode:** on by default: players with Microsoft accounts join as on vanilla. Horizon's own client cannot sign in to a Microsoft account yet, so Horizon players can only join with `online_mode = false`. An offline server lets anyone in under any name: keep the whitelist on.
- **Backups:** Horizon zips the world into `backups/` every 6 hours and keeps the last 12 (`[backups]`). They count against the server's disk.

Horizon's web panel and its `horizon-server update` command are not used here: the web panel only listens inside the container, and updates are done by reinstalling.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 25565   | TCP      | yes (primary allocation) |

## Requirements

- x86-64 nodes; Horizon publishes no Linux ARM build.
- About 1 GB of memory to run for a few players (Horizon measured 0.7 GB with one player and 1.1 GB with 50); more for large view distances.
- At least 2 GB of memory to install: preparing the game files runs Java exporters, and the installer gets the server's memory limit (or Wings' `installer_limits.memory`, 1024 MB by default, if higher). With 1.5 GB it fails. On one CPU core it takes about 5 minutes.
