# Barotrauma

Barotrauma is a 2D co-op submarine simulator - in space, on Jupiter's moon Europa - with survival horror and RPG elements. Steer your submarine, complete missions, fight monsters, fix leaks, operate machinery, man the guns and stay alert: danger lurks in the depths of Europa, and it may also lurk inside your submarine.

Native Calagopus egg for the Barotrauma dedicated server (Steam app `1026340`), installed anonymously through SteamCMD. It uses the **native Linux build**, which ships its own .NET runtime, and runs on the `ghcr.io/caloptreyx/steamcmd:debian` image.

## Capabilities

- Server update through SteamCMD on every start (Auto Update), optionally from a beta branch.
- Server name, password, player limit, server list visibility and ports are set from the panel on every start.
- The console shows the server log. It takes no commands: on a container terminal the server's console reader fills its input with garbage, so console input is turned off (see Admins).
- Stopping the server sends SIGINT; the server shuts down within seconds. Settings and permissions are written by the server whenever they change.
- Failed installations are reported to Calagopus as failed, with the reason.

## Server Ports

| Port  | Default | Protocol | Required |
|-------|---------|----------|----------|
| Game  | 27015   | UDP      | yes (primary allocation) |
| Query | 27016   | UDP      | yes, for the server list; must differ from the game port |

Add the query port as an additional allocation and set Query Port to it. The server only opens the query port (Steam queries, server list) when Public is on; with Public off, players join by IP and game port.

## Configuration

Name, password, max players (1-16), Public and the ports are passed to the server as launch arguments on every start; the server writes them to `serversettings.xml`. Everything else (game mode, respawning, karma, traitors, bots, allowed submarines, ...) is in `serversettings.xml`, written by the server with all its settings on the first start. Edit it while the server is stopped, or change the settings in game as an admin.

Extra Arguments takes further launch parameters, e.g. `-playstyle Serious` (`Serious`, `Casual`, `Roleplay`, `Rampage`, `SomethingDifferent`), `-karmaenabled true`, `-karmapreset Strict` (`Default`, `Strict`, `Custom`, from `Data/karmasettings.xml`), `-language German`.

## Admins

A dedicated server has no owner, and the panel console takes no commands. To make yourself an admin, stop the server and create `Data/clientpermissions.xml` (the server creates it once permissions are given):

```xml
<?xml version="1.0" encoding="utf-8"?>
<ClientPermissions>
  <Client name="YourName" accountid="76561198000000000" permissions="All" />
</ClientPermissions>
```

`accountid` is your SteamID64. Instead of `permissions="All"` you can use a rank preset from `Data/permissionpresets.xml`, e.g. `preset="Admin"`. Connected admins then manage the server in game (F3 console: `giverank <name> Moderator`, `kick`, `ban`, ...) and from the server settings menu. Bans are in `Data/bannedplayers.xml`.

## Saves

Multiplayer campaigns are saved in `Daedalic Entertainment GmbH/Barotrauma/Multiplayer/` in the server root. The game saves a campaign at the end of each round (when the submarine reaches the next location or returns to an outpost); stopping the server in the middle of a round keeps the last save, not the progress made in that round. Local mods go in `LocalMods/`.

## Requirements

About 0.2 GB of memory with no players and no round running; more during a round, with larger submarines and more players. Plan for 1-2 GB. The installation takes about 580 MB of disk.

## Sample ignore file for backups

Saved as `.pteroignore` in the server root, this limits backups to the settings, permissions, campaigns and mods:

```
# Ignore all
*
# Except the Barotrauma settings, campaigns and mods
!serversettings.xml
!Data/
!Daedalic Entertainment GmbH/
!LocalMods/
```
