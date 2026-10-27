# Arma 3

Experience true combat gameplay in a massive military sandbox.

Native Calagopus egg for the Arma 3 Linux dedicated server (Steam app `233780`). It runs in the [`ghcr.io/caloptreyx/games:arma3`](https://github.com/Caloptreyx/yolks) image, which updates the server and Steam Workshop mods on startup and starts the Headless Clients. Built after the Pterodactyl Arma 3 egg by [Red-Thirten](https://github.com/redthirten) and contributors; the image and install script are written for Calagopus.

## Capabilities

- Server updates on startup, on the `public`, `creatordlc` (Creator DLC) or `profiling` branch.
- Steam Workshop mods downloaded, updated and loaded on startup, from an Arma 3 Launcher preset export or the Additional Mods / Server-Side Mods / Optional Mods variables. Update checks ask the Steam Workshop API once for all mods, so unchanged mods cost no SteamCMD login.
- Downloaded mods are renamed to lowercase (the Linux server needs that) and their `.bikey` files copied to `keys/`. They are hard-link copies of SteamCMD's files, so an update only downloads what changed and takes no extra disk space.
- Optional mods: only their keys are installed, so players may join with or without them. Keys of optional mods that are removed from the list are deleted.
- Up to 5 Headless Clients started next to the server, and stopped with it.
- Server name, passwords and max players are written to `server.cfg` on every start, the BattlEye RCon port and password to `battleye/launch/beserver_x64.cfg` (and `beserver.cfg` for the 32-bit server). The rest of `server.cfg` stays yours.
- 64-bit server by default; 32-bit and profiling binaries selectable.
- Failed installations (for example a wrong Steam login) are reported to Calagopus as failed, with the reason.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game and voice | 2302 | UDP | yes (primary allocation) |
| Steam query (game port + 1) | 2303 | UDP | for the server browser |
| Steam (game port + 2) | 2304 | UDP | yes |
| BattlEye (game port + 4) | 2306 | UDP | yes |
| BattlEye RCon | 2301 | UDP | only for remote admin tools |

The ports after the game port are fixed offsets: add allocations for game port + 1, + 2 and + 4. The RCon port is set by the RCon Port variable and needs its own allocation only when used from outside.

## Requirements

> [!IMPORTANT]
> A real Steam account is required: anonymous login cannot install the Arma 3 server. Steam Guard must be turned off on that account. To download Workshop mods, the account must own Arma 3.

|           | Minimum | Recommended |
|-----------|---------|-------------|
| RAM       | 2048 MiB | 4096 MiB, plus about 2048 MiB per Headless Client |
| Storage   | 12 GiB | 30+ GiB, depending on mods and the `creatordlc` branch |
| CPU       | x86-64 | Fast cores; Arma 3 runs most of its simulation on one thread |

### Steam account

Set the `[Host]` Steam Username and Password variables. They are hidden from server owners, but stored in the panel, so use a dedicated account, not a personal one. As an admin you can set them as the egg's default values so every new server uses them.

If the account does not own Arma 3, Workshop downloads fail: set `[Host] Disable Mod Downloads/Updates` to `1` and upload mods by hand instead.

## Mods

- **Modlist File**: export a preset from the Arma 3 Launcher (Mods, Preset, Export to file) and upload the HTML file to the server root as `modlist.html`. Its Workshop mods are downloaded and loaded. Local mods and DLCs in the preset are ignored; add them to Additional Mods.
- **Additional Mods**: semicolon-separated folders, e.g. `@450814997;@463939057;vn;@mymod;`. `@<workshopID>` entries are downloaded; other names must be folders in the server root.
- **Server-Side Mods**: loaded with `-serverMod`, players do not need them.
- **Optional Mods**: `@<workshopID>` entries whose keys are installed, but which the server does not load.

Folder and file names must be lowercase and contain no spaces. For mods uploaded from Windows, enable `[Repair] Make Mod Files Lowercase` for one start.

With Automatic Updates off, mods that are not downloaded yet are still fetched, but installed mods are not updated.

### Creator DLC

Set Branch to `creatordlc`, restart (or reinstall) so the server downloads the CDLC files, then add the CDLC folders to Additional Mods: `vn` (S.O.G. Prairie Fire), `gm` (Global Mobilization), `ws` (Western Sahara), `csla` (CSLA Iron Curtain), `spe` (Spearhead 1944), `rf` (Reaction Forces), `ef` (Expeditionary Forces).

## Headless Clients

Set Headless Clients to 1-5. They connect to the server from inside the container and load the same mods. `server.cfg` must allow them, which the default file does:

```
headlessClients[] = { "127.0.0.1" };
localClient[] = { "127.0.0.1" };
```

Only missions written for Headless Clients benefit from them.

## Files

| Path | Contents |
|------|----------|
| `server.cfg` | Server settings, missions, difficulty |
| `basic.cfg` | Network tuning; recreated with defaults when deleted |
| `mpmissions/` | Mission files (`.pbo`) |
| `keys/` | Mod signature keys (`.bikey`) players are checked against |
| `battleye/launch/` | BattlEye configuration (RCon) |
| `.local/share/Arma 3 - Other Profiles/server/server.Arma3Profile` | Custom difficulty settings |
| `Steam/steamapps/workshop/` | SteamCMD's Workshop downloads; `@<workshopID>` folders are copies of these |

## Stopping

Stopping sends SIGINT; the server and the Headless Clients shut down.
