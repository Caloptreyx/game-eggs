# Terraria

Dig, fight, explore, build! Nothing is impossible in this action-packed adventure game. The world is your canvas and the ground itself is your paint.

Native Calagopus egg for the official Terraria dedicated server, downloaded from terraria.org (native Linux build with its own Mono runtime), or for [TShock](https://github.com/Pryaxis/TShock), the server mod with accounts, permissions, anti-cheat and plugins. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Two server types in one egg, selected by Server Type: `vanilla` (default) or `tshock`. Both use the same worlds, settings and port, so you can switch between them by changing Server Type and reinstalling.
- Any vanilla version (`latest` or a version such as `1.4.4.9`) and any TShock release (`latest` or a tag such as `v6.2.1`).
- TShock gets the .NET runtime it is built for (.NET 9 for TShock 6), installed into the server files.
- World, size, difficulty, seed, players, password, message of the day and anti-cheat are set from the panel and written to `serverconfig.txt` on every start. A missing world is created on start.
- Stopping the server sends `exit`, which saves the world first.
- Failed installations (for example a version that does not exist) are reported to Calagopus as failed, with the reason.

Vanilla console commands: `help`, `playing`, `save`, `kick <player>`, `ban <player>`, `say <message>`, `time`, `dawn`, `noon`, `dusk`, `midnight`, `settle`, `exit`, `exit-nosave`. TShock replaces them with its own commands (with or without `/`), e.g. `/playing`, `/save`, `/kick`, `/ban`, `/user`, `/group`, `/off`; `exit` also saves and stops TShock.

## Server Ports

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 7777    | TCP      | yes (primary allocation) |

The allocation's port is written to `serverconfig.txt` (and passed to TShock, whose `tshock/config.json` port would win otherwise). TShock's REST API (port 7878) is off by default; if you enable it in `tshock/config.json`, add its port as an additional allocation.

## Configuration

The panel variables are written to `serverconfig.txt` on every start; other [server config keys](https://terraria.wiki.gg/wiki/Server#Server_config_file) (`npcstream`, `language`, `banlist`, `journeypermission_*`, ...) can be added to the file and are kept.

Players need the same Terraria version as the server. The Steam client updates itself, so keep Terraria Version at `latest` and reinstall when a new version comes out. TShock releases support one Terraria version each (named in the release, e.g. "TShock 6.2.1 for Terraria 1.4.5.8") and usually follow a Terraria update by a few days.

Reinstalling (to update, or to change Server Type) replaces the server files only: each installation records its files in `.terraria-files` and removes the previous ones first, because TShock refuses to start next to the vanilla `TerrariaServer.exe`. Worlds, `serverconfig.txt`, `tshock/` and plugins you added to `ServerPlugins/` are kept.

## TShock

On the first start TShock prints a setup code (also in `tshock/setup-code.txt`). Join the game and type `/setup <code>`, then create your admin account as shown in the chat (`/user add <name> <password> owner`, `/login`), and finish with `/setup` to disable the code.

TShock settings are in `tshock/config.json` (server-side characters in `tshock/sscconfig.json`); the message of the day is `tshock/motd.txt`, and the Message of the Day variable has no effect with TShock. With a Server Password set, TShock warns that players with an account can still join through login-before-join and UUID login; set `DisableLoginBeforeJoin` and `DisableUUIDLogin` to `true` in `tshock/config.json` if only the password should count. Plugins (`.dll`) go into `ServerPlugins/`.

## Saves

Worlds are stored in `worlds/<World Name>.wld`, with backups of the previous saves as `.wld.bak` and `.wld.bak2`. A different World Name creates a new world; the old one is kept. To use an existing world, stop the server, upload the `.wld` file to `worlds/` and set World Name to its name (without `.wld`).

A new world is generated on the first start with a new World Name, which takes from a few seconds (small) to a few minutes (large); the server shows as running once it is done.

## Requirements

About 0.5-0.6 GB of memory for the vanilla server with a new small world and about 0.9 GB with a new medium world (no players), about 0.4 GB with TShock; more with players and for large worlds. Plan for 1-2 GB. The installation takes about 60 MB of disk for vanilla and about 190 MB for TShock (including the .NET runtime), plus about 3-14 MB per world and its backups.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the worlds and the settings:

```
# Ignore all
*
# Except the Terraria worlds, server settings and TShock data
!worlds/
!serverconfig.txt
!tshock/
```
