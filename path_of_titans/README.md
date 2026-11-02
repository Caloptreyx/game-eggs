# Path of Titans

Path of Titans is a multiplayer dinosaur survival game: survive as a dinosaur in an ecosystem full of AI creatures and up to 200 other players.

Native Calagopus egg for the official Linux server. It runs on the `ghcr.io/caloptreyx/games:pathoftitans` image, which updates the server on start with Alderon's AlderonGamesCmd and connects the panel console to the server over RCON. By hosting you agree to Alderon's [Server Hosting Agreement](https://alderongames.com/legal/server-hosting-agreement).

## Auth Token

The server files and the running server both need a hosting token from an Alderon Games account that owns Path of Titans. Generate it at <https://alderongames.com/oauth/hosting-token> and put it in **Auth Token** when you create the server. The installer downloads the server with it, so a wrong token fails the installation with the reason.

Generating a new token revokes every older one: a server still using an old token stops authenticating. Put the new token in every server and restart them. One token can run up to 100 servers.

## Capabilities

- Server update on every start (Auto Update), from the `production` or `demo-public-test` branch. If the update fails, the installed version starts.
- Console commands work: the server reads no console input, so the image sends each line to it over Source RCON on 127.0.0.1 and prints the answer. Commands are typed without `/` (`listplayers`, `teleportall talonspoint`); see Alderon's [admin commands](https://hosting.pathoftitans.wiki/guide/chat-commands#admin-commands).
- Stopping the server sends SIGINT, which Unreal servers answer by saving and shutting down.
- The Server GUID, which identifies the server in Alderon's server list, is generated on the first start and kept in `.pathoftitans-guid` when left empty. Set it to keep the GUID of a server you move here.
- Failed installations (an empty or wrong Auth Token) are reported to Calagopus as failed, with the reason.

## Settings

The Startup variables are applied on every start: the map, max players, server name, database, branch and RCON on the command line, and these keys in `PathOfTitans/Saved/Config/LinuxServer/Game.ini`:

| Variable | `Game.ini` key |
|----------|----------------|
| Server Password | `[/Script/PathOfTitans.IGameSession]` `ServerPassword` |
| Whitelist | `bEnforceWhitelist` (the Alderon IDs go one per line in `PathOfTitans/Saved/Whitelist.txt`) |
| Paid Users Only | `bServerPaidUsersOnly` |
| Discord Invite | `ServerDiscord` |
| (always) | `[SourceRCON]` `bEnabled=true`, for the console |

Everything else (growth, quests, nesting, allowed dinosaurs, restarts...) goes in `Game.ini` by hand: see Alderon's [server configurations](https://hosting.pathoftitans.wiki/setup/server-configurations). Stop the server before editing it. Bans are in `PathOfTitans/Saved/Bans.txt`, admin roles in `PathOfTitans/Saved/Config/LinuxServer/Commands.ini`. Mods are listed under `[PathOfTitans.Mods]` in `GameUserSettings.ini` (`EnabledMods=UGC_...`); a modded map also needs its name in Server Map.

**Server names** can't contain spaces: use underscores, which show as spaces.

**Database:** `Local` keeps characters in `PathOfTitans/Saved/DatabaseCache` on this server, where Calagopus backups cover them. `Remote` keeps them with Alderon and shares them between servers with the same token. Switching from Local to Remote replaces the local characters; back up first.

## RCON

RCON listens on the game port + 2 (RCON Port overrides it). With RCON Password empty, a random password is made on every start and only the console uses it. To use RCON tools or Discord bots from outside, set RCON Password (8 or more characters) and add the RCON port as an allocation.

## Server Ports

Path of Titans derives its other ports from the game port. Add allocations for them next to the game port:

| Port | Default | Protocol | Required |
|------|---------|----------|----------|
| Game | 7777 | UDP | yes (primary allocation) |
| Reserved | 7778 (game + 1) | TCP/UDP | for the server list |
| RCON | 7779 (game + 2) | TCP | only for RCON from outside |
| Stats | 7780 (game + 3) | TCP | |
| Query | 7781 (game + 4) | UDP | for the server list |

If the server is listed but joining from the list times out while direct connect works, set Server List IP to the address players should connect to.

## Requirements

- x86-64 nodes.
- Memory and disk grow with the map, the player count and mods; Alderon publishes no minimum. Watch the server's usage after the first start and size it from there.
