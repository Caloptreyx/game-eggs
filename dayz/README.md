# DayZ

> [!NOTE]
> This egg is for DayZ standalone only, not the DayZ mod for Arma 2 or Arma 3.

How long can you survive a post-apocalyptic world? A land overrun with an infected "zombie" population, where you compete with other survivors for limited resources. Will you team up with strangers and stay strong together? Or play as a lone wolf to avoid betrayal? This is DayZ – this is your story.

Native Calagopus egg, ported from the Pterodactyl DayZ egg by [Red-Thirten](https://github.com/lilkingjr1) and contributors (MIT License). It uses their `ghcr.io/ptero-eggs/games:dayz` image, which handles server and Workshop mod updates on startup.

## Capabilities

- Server updates on startup.
- Steam Workshop mods downloaded, updated and loaded on startup, from a DayZ Launcher modlist export or the Additional Mods / Server-Side Mods variables. Only mods in `@workshopID` form are updated automatically; their `.bikey` files are copied to `keys/`.
- Common `serverDZ.cfg` settings (name, passwords, max players, third person, time, ...) are set from the panel on every start.
- Failed installations (for example a wrong Steam login) are reported to Calagopus as failed, with the reason.

## Server Ports

| Port        | Default | Protocol | Required |
|-------------|---------|----------|----------|
| Game        | 2302    | UDP      | yes (primary allocation) |
| Steam Query | 27016   | UDP      | for the in-game server list |
| RCon        | 2305    | UDP      | only for remote admin tools |

Add the Steam Query port as an allocation and set the Steam Query Port variable to it.

## Requirements

> [!IMPORTANT]
> A real Steam account is required: anonymous login cannot install the DayZ server. Steam Guard must be turned off on that account.

|           | Minimum | Recommended |
|-----------|---------|-------------|
| RAM       | 4096 MiB | 8192 MiB |
| Storage   | 3072 MiB | 7168+ MiB, depending on mods |

### Steam account

Set the `[Host]` Steam Username and Password variables. They are hidden from server owners, but stored in the panel, so use a dedicated account, not a personal one. As an admin you can set them as the egg's default values so every new server uses them.

The account does not need to own DayZ. If it does, the vanilla mission files update with the server and Workshop mods can be downloaded. If it does not:

- The installer downloads the vanilla missions (Chernarus, Livonia, Sakhal) from [BohemiaInteractive/DayZ-Central-Economy](https://github.com/BohemiaInteractive/DayZ-Central-Economy). To update them later, delete `mpmissions/` and reinstall; other files are kept.
- Workshop mod downloads fail. Set `[Host] Disable Mod Downloads/Updates` to `1` and upload mods manually.

## Map

The map is set by `template` in `serverDZ.cfg`:

| Map       | Template |
|-----------|----------|
| Chernarus | `dayzOffline.chernarusplus` |
| Livonia   | `dayzOffline.enoch` |
| Sakhal    | `dayzOffline.sakhal` |

## Running with Steam Guard enabled

> [!CAUTION]
> Not recommended. Every update and every mod download needs a new Steam Guard code.

1. Set `[Host] Skip Game Server Install` and `[Repair] Validate Server Files` to `1`, then install.
2. Start the server. When the console stops at `Loading Steam API...OK`, enter a Steam Guard code and press Enter. `Two-factor code:OK` means you are logged in and the server downloads.
3. Repeat with a new code for every mod.
4. Afterwards, turn off `[Repair] Validate Server Files`, and turn off Automatic Updates until you need them.
