# Horizon

[Horizon](https://playhorizon.dev/docs/hosting)'s server for Minecraft: Java Edition 26.3. `horizon-server` runs Horizon's own engine, a native program, instead of Mojang's Java server, with vanilla's settings, world format and console commands. Vanilla 26.3 clients and Horizon clients can join.

Native Calagopus egg. It runs on the `ghcr.io/caloptreyx/games:horizon` image, which downloads and updates `horizon-server` on start from Horizon's GitHub releases (checked against the SHA-256 GitHub publishes) and prepares its game files. The installer itself downloads nothing.

## Game files

Horizon reads Minecraft's data from game files prepared from Minecraft 26.3. By default (Prepare Game Files `1`) the image prepares them whenever the installed Horizon build has none yet: it downloads Mojang's 26.3 version file, client, client libraries, the non-sound assets and server jar, each checked against Mojang's checksum, and runs `horizon-prepare` with the image's Java 25. The downloads are removed afterwards; only the prepared files are kept, in `horizon/.local/horizon-client/26.3`. The first start takes a few minutes for this (about 3 on one CPU core) and fits in 1 GB of memory.

Horizon itself never downloads Mojang's files: it expects them to come from your own copy of Minecraft. To keep it that way, set Prepare Game Files to `0`, prepare the files on your computer with the Horizon launcher or `horizon-prepare --prepare --server-jar server.jar`, and upload the prepared `26.3` folder as `resources/` in the server's root. A folder there takes precedence.

## Updates

- **Horizon Version:** `latest` (the current Nightly, the only build Horizon ships) or a tag from [Horizon's releases](https://github.com/horizon-rust/horizon/releases) such as `v0.2.97` or `nightly-2026-10-29`.
- **Auto Update `1`** (default): every start asks GitHub for the newest build of that version and installs it, then prepares the game files for it. Nothing is downloaded when the installed build is current. If GitHub can't be reached, the installed build starts.
- **Auto Update `0`:** the installed build is kept. Changing Horizon Version still installs the new one on the next start.
- **Reinstall** downloads Horizon and prepares the game files again on the next start. Worlds and settings are kept.

The installed build is in `horizon/.horizon-release`. Horizon's own `horizon-server update` and its update notices are turned off; they need Horizon's installer layout.

## Settings

All settings live in `server.toml`; `horizon-server` writes `server.properties` from it on every start, so edit `server.toml`, not `server.properties`. On every start the image writes the Startup variables below into `server.toml` with `horizon-server config set`, which checks them as the server does: a value it refuses stops the start with the reason in the console. **A variable left empty is not touched**, so `server.toml` (and in-game changes such as `/whitelist off`, which Horizon copies back into it) keeps its value. The allocation's port is always written to `server.port`.

| Variable | `server.toml` | Notes |
|----------|---------------|-------|
| Online Mode | `server.online_mode` | `true` checks Minecraft accounts. Horizon's own client can't sign in to Microsoft accounts yet, so Horizon players need `false`, which lets anyone join under any name: keep the whitelist on then. |
| Whitelist | `server.whitelist` | On by default in Horizon. |
| Whitelisted Players | `whitelist.json` | Comma-separated names added on every start (after Online Mode, so they get the right UUIDs). Remove a name here too when you take it off the whitelist. |
| Operators | `ops.json` | Comma-separated names made operators on every start. |
| MOTD, Max Players, Game Mode, Difficulty, View Distance | `server.*`, `world.*` | |
| Horizon Backups | `backups.every` | Default `0`: Horizon's own world zips in `backups/` are off, since Calagopus backups cover the world and Horizon's use the server's disk. `30m`, `6h`, `1d`... turns them on (it keeps the last 12). |
| Pregeneration | `properties.horizon-pregen` | `true` enables Horizon's `pregen` console command, which generates the world around spawn ahead of the players: `pregen start 2500`. |

On the first start an existing vanilla `server.properties` is imported into `server.toml`, so a vanilla server folder can be switched to Horizon.

## Capabilities

- Calagopus's EULA prompt: on first start the console asks to accept the Minecraft EULA and writes `eula.txt`.
- Console commands go to the game, as on vanilla. Stopping the server sends `stop`, which saves the world.
- If the game crashes, `horizon-server` restarts it by itself (after 5 s, then waiting longer, up to 5 crashes in 10 minutes).
- Horizon's web panel is not reachable: it only listens inside the container. Calagopus's console, stats and backups cover it.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 25565   | TCP      | yes (primary allocation) |

## Requirements

- x86-64 nodes; Horizon publishes no Linux ARM build.
- About 1 GB of memory for a few players (Horizon measured 0.7 GB with one player and 1.1 GB with 50), which is also enough to prepare the game files; more for large view distances.
- About 400 MB of disk for Horizon and the prepared files, plus the world.
