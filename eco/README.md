# Eco

Eco is a community-based game where players develop laws, government and an economy to determine the success of their world, and must build a civilization without destroying the ecosystem it depends on.

Native Calagopus egg for the Eco dedicated server (Steam app `739590`), installed anonymously through SteamCMD. It uses the **native Linux build**, so no Wine is needed. It runs on the `ghcr.io/caloptreyx/steamcmd:debian` image; the libraries the server needs beyond it (ICU and libgdiplus) are installed with the server.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- Server name, password, server browser visibility and the Strange Loop Games server token are set from the panel; the ports follow the allocations.
- Console input: chat commands such as `/manage save`, `/manage kick`, `/manage admin` or `/?` are typed in the panel console and passed to the server through its RCON interface, which prints the reply.
- Stopping the server saves the world first.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 3000    | UDP      | yes (primary allocation) |
| Web   | 3001    | TCP      | yes, primary + 1 (web interface and API, used by the game client) |
| RCON  | 3002    | TCP      | only for remote RCON tools, primary + 2 |
| Steam | 3003    | UDP      | recommended, primary + 3 |

Allocate the port following the primary allocation (and the next two when used) as additional allocations; the egg writes them into `Configs/Network.eco` as fixed offsets from the game port. The console reaches RCON locally, so the RCON port only needs an allocation for remote RCON tools. In offline mode (no Server Token) the server does not open the Steam port.

## Server Token

Since Eco 11 a server must authenticate with Strange Loop Games on start. Log in on https://play.eco with an account that owns Eco, open the account page and generate a token under **Server Authentication**, then paste it into the Server Token variable. Without a token the server starts in offline mode: it is not registered in the server browser and user data is not updated from Strange Loop Games. A wrong token stops the server on start with `Authentication to Strange Cloud failed`.

## Configuration

Server Name, Server Password, Public Server, the RCON password and the ports are written into `Configs/Network.eco` on every start. UPnP is turned off there, as it cannot work in a container. The RCON Password (at least 8 characters) is required: the panel console sends its commands through RCON.

Every other setting lives in `Configs/`: the server reads `<name>.eco` when it exists and the defaults in `<name>.eco.template` otherwise. To change a setting, copy the template to `<name>.eco` (e.g. `WorldGenerator.eco.template` to `WorldGenerator.eco`) and edit it while the server is stopped. World generation settings only apply to a new world.

## Admins

Admins are added from the console with `/manage admin <name or SteamID64 or SLG id>`, or listed in `Configs/Users.eco`.

## Saves

The world is stored in `Storage/` (`Game.eco`, `Game.db` and journal files). The server saves periodically, on `/manage save` and on shutdown. To start a new world, stop the server and delete `Storage/`.

## Requirements

Generating a new world (default size) took about 60 seconds and peaked at about 2.9 GB of memory; a restarted server with that world and no players used about 2 GB. Plan for 4 GB, more for larger worlds and many players. The installation takes about 1.2 GB of disk; a new world about 40 MB with its backup.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the world and the configuration:

```
# Ignore all
*
# Except the world and the configuration
!Storage/
!Configs/
```
