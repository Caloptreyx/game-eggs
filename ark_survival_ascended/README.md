# ARK: Survival Ascended

ARK is reimagined from the ground up in Unreal Engine 5. Form a tribe, tame and breed hundreds of unique dinosaurs and primeval creatures, explore, craft, build and fight your way to the top of the food chain.

Native Calagopus egg. The dedicated server (Steam app `2430930`) only exists for Windows. It is installed anonymously through SteamCMD and runs through Proton in the `ghcr.io/ptero-eggs/steamcmd:proton` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Server name, passwords, PvE and max players are written to `GameUserSettings.ini` by the panel, so names may contain quotes, `$` and brackets.
- The server log is streamed to the console, and console commands are sent to the server over RCON, e.g. `ListPlayers`, `SaveWorld`, `Broadcast Hello`, `KickPlayer <EOS ID>`.
- Stopping the server sends `DoExit`, which saves the world before exiting.
- CurseForge mods by project ID; the server downloads and updates them itself.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 7777    | UDP      | yes (primary allocation) |
| RCON  | 27020   | TCP      | only for remote admin tools; the panel console uses it locally |

## Requirements

|         | Minimum | Recommended |
|---------|---------|-------------|
| RAM     | 12 GB   | 16 GB or more, depending on players, map and mods |
| Storage | 15 GB   | 30 GB or more; saves and mods grow over time |
| CPU     | x86-64 with AVX | Proxmox VMs need the `host` CPU type |

An empty server on The Island uses about 10 GB of memory once it is up. With less memory the server is killed during startup.

Startup takes a minute or two. The panel's stop only works once the server is up, because it goes through RCON; before that, the panel falls back to killing the server, which is harmless because nothing has been played yet.

## Configuration

| File | Contents |
|------|----------|
| `ShooterGame/Saved/Config/WindowsServer/GameUserSettings.ini` | Server settings (`[ServerSettings]`, rates, ...) |
| `ShooterGame/Saved/Config/WindowsServer/Game.ini` | Advanced gameplay settings |

The server rewrites these files when it stops, so edit them only while it is stopped. The settings available as panel variables are written on every start, so change those in the panel.

Map names are the ones with the `_WP` suffix, e.g. `TheIsland_WP`, `ScorchedEarth_WP`, `TheCenter_WP`, `Aberration_WP`, `Extinction_WP`, `Ragnarok_WP`, `Valguero_WP`, `LostColony_WP`. Players need to own DLC maps to join them.

## Saves

The world is saved in `ShooterGame/Saved/SavedArks/<map>/`, with player and tribe files alongside it.

## Troubleshooting

- Crash reports are written to `ShooterGame/Saved/Crashes/`.
- If the server stops during startup without an error, check that it has enough memory.
- Try a start without mods on a fresh install to rule out mod problems.
