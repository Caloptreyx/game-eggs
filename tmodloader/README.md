# Terraria: tModLoader

[tModLoader](https://github.com/tModLoader/tModLoader) is the official mod loader for Terraria (available free on Steam for Terraria owners): it runs thousands of content mods such as Calamity, Thorium and Magic Storage.

Native Calagopus egg for a tModLoader dedicated server from the tModLoader GitHub releases (`tModLoader.zip`), with the .NET runtime its runtimeconfig asks for in `.dotnet/` and the ICU libraries in `.icu/`. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image. For vanilla Terraria or TShock, use the Terraria egg.

## Capabilities

- Install of the latest release or a fixed tag (tModLoader Version); reinstall to update. `saves/`, `mods/` and `serverconfig.txt` are kept.
- World, size, difficulty, players, password and message of the day are set from the panel on every start; a missing world is created on start.
- Console commands go straight to the server: `help`, `playing`, `save`, `kick <player>`, `ban <player>`, `say <message>`, `time`, `dawn`, `noon`, `dusk`, `midnight`, `settle`, `exit`.
- Stopping the server sends `exit`, which saves the world first.
- Failed installations are reported to Calagopus as failed, with the reason.

## Mods

Copy the mods' `.tmod` files into `mods/` and list the enabled ones in `mods/enabled.json` (a JSON array of mod names, e.g. `["CalamityMod", "MagicStorage"]`). A quick way to get both: enable the mods in your own tModLoader client and copy the `.tmod` files and `enabled.json` from its mods folder (Workshop mods are in the Steam Workshop folder of app 1281930). Players need the same mods; tModLoader offers to download them from the server when they join. Mod settings are in `saves/ModConfigs/`.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 7777    | TCP      | yes (primary allocation) |

## Configuration

`serverconfig.txt` is the documented example shipped with tModLoader; the panel writes the difficulty into it, and the other panel settings are launch arguments, which win over the file. Other settings (`npcstream`, `secure`, `language`, `journeypermission_*`, ...) can be enabled there.

## Saves

Worlds are in `saves/Worlds/` (`<name>.wld` with the modded data in `<name>.twld`, plus backups). The world is saved on `save`, on stop and when the game autosaves.

## Requirements

About 950 MB of memory for a small world without mods; plan for 2 GB, and 4 GB or more with large mods. The installation takes about 285 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to worlds, mods and configuration:

```
# Ignore all
*
# Except worlds, mods and the config
!saves/
!mods/
!serverconfig.txt
```
