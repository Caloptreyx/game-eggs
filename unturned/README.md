# Unturned

You're one of the few survivors left in the zombie apocalypse. Scavenge for supplies, build a base, team up with other survivors or fight them.

Native Calagopus egg for the Unturned dedicated server (Steam app `1110390`), installed anonymously through SteamCMD. It uses the native Linux build and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update).
- Name, password, login token, map, max players, difficulty, perspective, PvE, cheats and owner are panel variables, written to `Commands.dat` on every start.
- The console takes server commands, e.g. `help`, `players`, `say <message>`, `save`, `kick <player>`, `admin <steamid>`, `shutdown`. The responses appear on the console.
- Stopping the server sends `shutdown`, which saves the world and the players before the server exits.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Query | 27015   | UDP      | yes (primary allocation) |
| Game  | 27016   | UDP      | yes, primary + 1 |

The primary allocation answers the Steam server queries; the server also uses the next port for game traffic, so allocate it as an additional allocation. For more servers on one host, keep two ports per server (27015, 27017, 27019, ...).

## Configuration

Each server keeps its settings and saves in `Servers/<Server Folder>/` (`Servers/Default/` by default).

- `Server/Commands.dat`: startup commands, one per line. The panel sets `Name`, `Port`, `Password`, `GSLT`, `Map`, `MaxPlayers`, `Mode`, `Perspective`, `PvE`, `Cheats` and `Owner` there on every start and replaces any such lines; other commands you add (e.g. `Welcome`, `Chatrate`, `Timeout`, `Log`) are kept. Unturned ignores these settings when given on the command line.
- `Config.txt`: gameplay rules (loot, zombies, decay, ...) and the server list texts (`Browser` section), created on the first start. Settings left empty use the defaults of the Difficulty.
- `WorkshopDownloadConfig.json`: Workshop mods and maps (`File_IDs`), downloaded on start. Workshop maps (e.g. Hawaii, `1753134636`) are set with the Map variable after adding them here.
- `Server/Adminlist.dat`, `Blacklist.dat`, `Whitelist.dat`: admins, bans and permits, managed with the console commands.

RocketMod and OpenMod are not part of this egg.

## Game Server Login Token

Without a token the server is not listed in the internet server list and cannot be joined over the internet, only by its Server Code (printed on start, changes on every start) or a Steam invite. Create a token at [steamcommunity.com/dev/managegameservers](https://steamcommunity.com/dev/managegameservers) with the game's app ID `304930` (not the server's), and enter it in Game Server Login Token. With a token the Server Code stays the same. See the [official guide](https://docs.smartlydressedgames.com/en/stable/servers/game-server-login-tokens.html).

With an invalid or revoked token the server stops at `Waiting for Steam servers...` after logging `Game Server Login Token (GSLT) is invalid`, and never finishes starting. Clear or replace the token and restart.

## Console

The server reads commands from its terminal, which the panel provides; the server counts as started once the map is loaded (`Loading level: 100%`). Stopping the server sends `shutdown`.

## Saves

The world is saved in `Servers/<Server Folder>/Level/<map>/`. The server has no autosave: it saves on `save` and on `shutdown` (panel stop). Killing the server loses everything since the last save.

## Requirements

About 1 GB of memory on PEI without players; larger maps, players and Workshop content need more. The installation takes about 2 GB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to your config and saves:

```
# Ignore all
*
# Except the Unturned server folders (config and saves)
!Servers/
```
