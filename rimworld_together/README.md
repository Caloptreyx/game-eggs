# RimWorld Together

A community multiplayer mod for RimWorld: every player runs their own colony on a shared planet and can trade, visit, raid and send aid to each other.

Native Calagopus egg for the official server from the [RimWorld Together GitHub releases](https://github.com/RimWorld-Together/Rimworld-Together/releases) (self-contained Linux x64 build). It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image. Players need the RimWorld Together mod from the Steam Workshop, in the same version as the server.

## Capabilities

- Installs the newest release, or a fixed one (RimWorld Together Version, e.g. `26.10.4.1`). Reinstall to change versions; only the server executable is replaced, the world, players, saves and configs are kept.
- Downloads are checked against the SHA-256 digest GitHub publishes for the release asset.
- Server name, description, max players and public listing are written to `Configs/ServerConfig.json` on every start; the port comes from the primary allocation.
- The Server Password is stored hashed in `Configs/PasswordConfig.json` on every start, the same way the `setpassword` console command does. Empty means no password.
- Console commands go straight to the server, e.g. `help`, `list`, `op <player>`, `backup`. Stopping the server sends `quit`.
- Failed installations (unknown version, GitHub unreachable, corrupt download) are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 25555   | TCP      | yes (primary allocation) |

## Server browser

With Public on, the server is listed in the in-game RimWorld Together server browser. With it off, the server still reports to the browser service, but as a private server that is not listed. Both need `EnableServerTelemetry` (on by default) in `Configs/ServerConfig.json`; set it to `false` to not contact the browser service at all.

## Files

| Path | Contents |
|------|----------|
| `Configs/ServerConfig.json` | Server settings (the panel manages Port, Name, Description, MaxPlayers, EnableServerBrowser) |
| `Configs/DifficultyConfig.json`, `ModConfig.json`, `BackupConfig.json`, ... | Gameplay, mod enforcement, backups and chat settings |
| `Assets/` | World, players and saves |
| `Backups/` | Automatic backups (hourly by default) |
| `Logs/` | Server logs |

## Requirements

- About 100 MB of disk, plus the world and backups.
- An empty server uses about 30 MB of RAM; usage grows with players and world size.

## Open World

`egg-rimworld-open-world.yml` runs the server of [Open World](https://github.com/D12-Dev/OpenWorld), an older RimWorld multiplayer mod (its last release, v1.0.4 from 2023, is for RimWorld 1.4; RimWorld Together above is the actively maintained alternative). The installer takes the `linux-x64` server from the GitHub release and adds the .NET 8 runtime in `.dotnet/`; the server targets the end-of-life .NET Core 3.1 and runs on .NET 8 through roll-forward, with invariant globalization (the image has no ICU).

- One TCP port (default 25555), the primary allocation; the panel writes it, the player limit and the mod and whitelist switches into `Data/Config.json` on every start (the installer creates that file).
- Console lines are server commands (`help` lists them, e.g. `backup`, `wipe`, `changepass`). Stopping sends `shutdown`.
- Players, saves, settlements and backups are kept in the folders of the same names; mod lists in `Mods enforced/`, `Mods whitelisted/` and `Mods blacklisted/`.
- About 70 MB of disk with the .NET runtime.
